---
description: How the caching layers in WordPress fit together, from the object cache API and transients to page caching and OPcache.
---

# Caching In WordPress

"Caching" in WordPress is not one thing. A typical site has several independent caches, each storing a different kind of result at a different point in the request. Most confusion, and most misconfiguration, comes from treating them as interchangeable. In this article we walk through each layer, show how to use the object cache API correctly, and explain where a file-based persistent object cache such as Docket Cache sits among them.

## The Layers At A Glance

| Layer | What it stores | Where it lives | Survives the request |
| --- | --- | --- | --- |
| OPcache | Compiled PHP bytecode | Shared memory of the PHP process | Yes |
| Object cache (default) | PHP values keyed by name and group | Memory of the current request | No |
| Object cache (persistent) | The same values | Redis, Memcached, files and so on | Yes |
| Transients | Values with an optional lifetime | Options table, or the object cache | Yes |
| Page cache | Complete HTML responses | Disk, memory or a reverse proxy | Yes |

The layers complement each other. A page cache avoids running WordPress at all for requests it can answer. The object cache makes the requests that do reach WordPress cheaper. OPcache makes the PHP code behind both faster to load.

## The Object Cache

WordPress core wraps almost every expensive lookup in the object cache: posts, post meta, terms, users, options and, since WordPress 6.1, the results of `WP_Query` database queries. The API is a small set of functions backed by a global `WP_Object_Cache` instance.

Every entry is identified by a key and a group. The group is a namespace, so the key `42` in group `posts` is unrelated to the key `42` in group `users`.

| Function | Purpose |
| --- | --- |
| `wp_cache_get( $key, $group, $force, &$found )` | Read a value |
| `wp_cache_set( $key, $data, $group, $expire )` | Write a value, replacing any existing one |
| `wp_cache_add( $key, $data, $group, $expire )` | Write only if the key does not exist yet |
| `wp_cache_delete( $key, $group )` | Remove a single entry |
| `wp_cache_flush()` | Remove everything |

### Non-persistent By Default

Out of the box, `WP_Object_Cache` keeps its data in a PHP array. The cache is created at the start of a request and discarded at the end. That still helps, because the same post or option is often requested many times while one page is built, but nothing carries over to the next visitor. Every request starts cold and repeats the same database queries.

### Persistent With A Drop-in

To keep cached values between requests, WordPress looks for a file named `object-cache.php` in the `wp-content` directory. If it exists, WordPress loads it instead of its built-in implementation, and that file is free to define the `wp_cache_*` functions against any storage backend. This file is called a drop-in. It is not a normal plugin: it loads very early, before plugins, and only one can be present at a time.

When a drop-in is active, `wp_using_ext_object_cache()` returns `true`. Core and plugins use that function to change their behaviour, most notably for transients, as described below. Since WordPress 6.1, the Site Health screen also reports whether a persistent object cache is in use and recommends one when the site would benefit.

With WP-CLI you can check which backend is active:

```shell
wp cache type
```

### Using The API Correctly

The standard pattern is read, fall back, write:

```php
function myplugin_get_report( int $year ) {
    $key    = 'report_' . $year;
    $report = wp_cache_get( $key, 'myplugin', false, $found );

    if ( ! $found ) {
        $report = myplugin_build_report( $year ); // The slow part.
        wp_cache_set( $key, $report, 'myplugin', HOUR_IN_SECONDS );
    }

    return $report;
}
```

Two details matter here. First, `wp_cache_get()` returns `false` on a miss, which is indistinguishable from a cached `false`, `0` or empty result. The fourth parameter, `$found`, is set by reference and tells you whether the key really existed. Without it, a legitimately empty result is rebuilt on every request. Second, always use your own group name so that your keys cannot collide with core or with other plugins.

When you need several entries, fetch them in one call. `wp_cache_get_multiple()` was added in WordPress 5.5, and the matching `wp_cache_set_multiple()`, `wp_cache_add_multiple()` and `wp_cache_delete_multiple()` in 6.0. Backends with a network round trip per lookup benefit the most.

```php
$keys   = array( 'report_2024', 'report_2025', 'report_2026' );
$values = wp_cache_get_multiple( $keys, 'myplugin' );

foreach ( $values as $key => $value ) {
    if ( false === $value ) {
        // Not cached: rebuild this one.
    }
}
```

### Invalidation

Delete or overwrite the entry at the moment the underlying data changes, rather than waiting for it to expire. For a single entry that is `wp_cache_delete()`. To clear everything your plugin has cached, WordPress 6.1 introduced `wp_cache_flush_group()`. Not every backend can flush a single group, so check first with `wp_cache_supports()`, also added in 6.1:

```php
function myplugin_clear_cache() {
    if ( wp_cache_supports( 'flush_group' ) ) {
        wp_cache_flush_group( 'myplugin' );
        return;
    }

    // Fallback: delete the keys you know about.
    foreach ( array( 'report_2024', 'report_2025', 'report_2026' ) as $key ) {
        wp_cache_delete( $key, 'myplugin' );
    }
}
```

`wp_cache_supports()` accepts `add_multiple`, `set_multiple`, `get_multiple`, `delete_multiple`, `flush_runtime` and `flush_group`.

{% hint style="info" %}
Avoid calling `wp_cache_flush()` from plugin code. It empties the cache for the whole site, and on a busy site every visitor then pays to rebuild it at the same time.
{% endhint %}

## Transients

A transient is a named value with an optional lifetime, set with `set_transient()` and read with `get_transient()`. Transients exist to give plugin and theme authors something that persists between requests on every site, whether or not a persistent object cache is installed.

```php
$rates = get_transient( 'myplugin_rates' );

if ( false === $rates ) {
    $response = wp_remote_get( 'https://api.example.com/rates' );

    if ( ! is_wp_error( $response ) ) {
        $rates = json_decode( wp_remote_retrieve_body( $response ), true );
        set_transient( 'myplugin_rates', $rates, 15 * MINUTE_IN_SECONDS );
    }
}
```

Where the value ends up depends on the site:

- Without a persistent object cache, the transient is written to the options table as a `_transient_{name}` row, plus a `_transient_timeout_{name}` row when it has an expiry.
- With a persistent object cache, `set_transient()` calls `wp_cache_set()` in the `transient` group instead and the database is not touched. Site transients use the `site-transient` group.

This switch has practical consequences:

- A transient is never guaranteed to exist until its expiry. A cache flush or an eviction can remove it early, so the code must always be able to rebuild the value.
- In the database, a transient without an expiry is stored as an autoloaded option and is therefore loaded on every request. Give transients an expiry unless they are small and genuinely needed everywhere.
- `get_transient()` returns `false` on a miss and has no `$found` equivalent, so do not store a bare `false`. Store an empty array or another sentinel value instead.
- Transient names should be 172 characters or fewer.

## Options And The alloptions Cache

Options have their own caching built on top of the object cache. Early in each request, `wp_load_alloptions()` reads every autoloaded option in a single query and stores the whole set under the key `alloptions` in the `options` group. Any later `get_option()` call for an autoloaded option is answered from that array. Options that are not autoloaded are queried on first use and cached individually in the same group.

This is efficient as long as the autoloaded set stays small. It becomes a problem when plugins store large blobs or rarely used data as autoloaded options, because the whole array is loaded on every request, and with a persistent object cache it is also read from and written back to the backend as one large value. Since WordPress 6.6, core decides for itself whether an option should autoload when the caller does not specify, and it keeps very large values out of the autoloaded set by default.

When you add an option that is only needed on a few screens, say so explicitly:

```php
add_option( 'myplugin_import_log', $log, '', false );
```

## Page Caching

A page cache stores the finished HTML of a response and serves it for later requests to the same URL. On a hit, little or no WordPress code runs, which is why it gives the largest improvement of any layer for anonymous traffic.

Inside WordPress, page caching plugins hook in through another drop-in, `wp-content/advanced-cache.php`, which is loaded very early when the `WP_CACHE` constant is `true`. Outside WordPress, the same job can be done by the web server, a reverse proxy or a CDN.

The limitation is that a cached page is the same for everyone who receives it. Logged-in users, carts, checkouts, the dashboard, REST API and AJAX requests are normally excluded. Those requests run the full WordPress stack, and that is exactly where the object cache does its work. A site with many logged-in users or an active shop depends far more on the object cache than a brochure site does.

## OPcache

OPcache is a PHP extension, not a WordPress feature. It keeps the compiled bytecode of PHP scripts in shared memory so that PHP does not have to read and compile the same files on every request. It speeds up loading WordPress core, plugins and themes, but it knows nothing about posts, options or queries. It caches code, not data.

## Where Docket Cache Fits

Docket Cache is a persistent object cache. It installs the `object-cache.php` drop-in, so everything described above about a persistent backend applies: core object caching survives between requests, and transients move out of the options table into the cache.

What differs is the storage. Instead of sending values to a Redis or Memcached server, or writing them to files with `serialize` and reading them back with `unserialize`, Docket Cache writes each cached object as plain PHP code. Because the cache files are ordinary PHP files, OPcache compiles them and keeps them in shared memory, and reading a cached object becomes a matter of loading an already compiled script. In effect, the data layer borrows the mechanism PHP already uses for code. It needs no extra service, which makes it suitable for shared hosting where Redis and Memcached are not available. It still works when OPcache is not available, only slower, because the files are then read from disk.

A few behaviours are worth knowing when you write code that runs on top of it:

- An entry stored with no expiry, or an expiry of `0`, is given a default lifespan set by `DOCKET_CACHE_MAXTTL`, which is four days unless changed.
- The groups `counts`, `plugins` and `themes` are not stored by default. The list is controlled by `DOCKET_CACHE_IGNORED_GROUPS`.
- If a plugin misuses transients, for example by storing very large values with no expiry, `DOCKET_CACHE_TRANSIENTDB` keeps transients in the database rather than the object cache.

Docket Cache replaces other object cache plugins, since only one `object-cache.php` can exist, but it works alongside a page cache. The two solve different problems and are best used together.

## Practical Guidelines

- Treat every cached value as disposable. The site must produce correct results with an empty cache, only more slowly.
- Use the `$found` parameter of `wp_cache_get()` so that empty results are cached as well.
- Namespace your entries with your own group, and invalidate them when the source data changes.
- Set an expiry as a safety net, even when you also invalidate explicitly.
- Choose the object cache for data derived from the database, and transients for data that must outlive the request on any site, such as remote API responses.
- Keep autoloaded options small, and pass `false` for autoload when an option is rarely needed.
- Test with the persistent object cache both enabled and disabled. Stale data bugs often only appear once values survive between requests.
