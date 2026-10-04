---
description: Frequently Asked Questions
---

# FAQ

## What is Object Caching in WordPress?

Object caching stores database query results so they can be retrieved quickly the next time they are needed.

Cached data is served directly from the cache rather than making repeated database queries. This reduces the load on your server and makes your site faster.

In simple terms, object caching keeps frequently used data in a location where it can be accessed more quickly.

## What is Docket Cache in Object Caching?

By default, the WordPress object cache is non-persistent. This means cached data is stored in memory only for the duration of a single page request. It is not kept between page loads. To make it persistent, the object cache must be stored on disk.

Docket Cache does more than just store the object cache — it converts cached data into plain PHP code. This is faster because WordPress can use the cache directly without additional processing.

## What is OPcache in Docket Cache?

OPcache is a caching engine built into PHP. It improves performance by storing precompiled script bytecode in shared memory, so PHP does not need to load and parse scripts on each request.

Docket Cache converts the object cache into plain PHP code. When reading and writing cache data, it uses OPcache directly, which results in faster data retrieval and better performance.

## What is the Cronbot Service in Docket Cache?

Cronbot is an external service that pings your website every hour to keep WordPress Cron running actively.

This service is optional and is not required. By default, it is not connected to the [end-point server](https://cronbot.docketcache.com/). Only the Cronbot screen is enabled by default, which shows the cron events of your site; the service starts only after you click Connect on that screen. You can disable it entirely on the configuration page.

## What is the Garbage Collector in Docket Cache?

The Garbage Collector is a scheduled event that runs every 5 minutes to monitor cache files, clean up expired entries, and collect statistics.

## What is a RAM disk in Docket Cache?

A RAM disk uses your server's memory (RAM) to act as a storage drive. It can be a hardware device or a virtual disk.

Reading and writing data from RAM is much faster than from an SSD. Storing Docket Cache files on a RAM disk can greatly improve performance.

Please note that creating a RAM disk requires server administrator access (root), so this option is not available on shared hosting.

Here is an example command to create and use a RAM disk with Docket Cache:

```shell
$ cd wp-content/
$ sudo mount -t tmpfs -o size=500m tmpfs ./cache/docket-cache
```

To mount the cache path automatically when the server starts, update your `/etc/fstab` file.

For more information about RAM disks:

1. [How to Easily Create a RAM Disk](https://www.linuxbabe.com/command-line/create-ramdisk-linux)
2. [What Is /dev/shm and Its Practical Usage](https://www.cyberciti.biz/tips/what-is-devshm-and-its-practical-usage.html)
3. [Creating a Filesystem in RAM](https://www.cyberciti.biz/faq/howto-create-linux-ram-disk-filesystem/)

On Windows, create a RAM disk and set the [DOCKET\_CACHE\_PATH](constants.md#docket\_cache\_path) to point to the RAM disk drive.

## What is the minimum RAM required for shared hosting?

By default, WordPress sets the memory limit to 256 MB. When combined with MySQL and the web server, you need more than 256 MB. If your hosting plan only provides 256 MB in total, this is not enough, and Docket Cache will not be able to improve your site's performance.

## How is Docket Cache different from other object cache plugins?

Docket Cache is an Object Cache Accelerator. It optimises caching by handling post queries, comment counting, WordPress translations, and more before storing the object cache.

## Can I use it alongside other cache plugins?

Yes and no. You can use it together with a page caching plugin, but not with another object cache plugin.

## Can I use it with LiteSpeed Cache?

Yes. The LiteSpeed Cache plugin includes its own Object Cache feature. It may display a notice asking you to disable Docket Cache. Simply turn off the LiteSpeed Cache Object Cache to use Docket Cache instead.

## Can I use Docket Cache on a busy WooCommerce store?

It is not recommended for high-traffic WooCommerce stores. Docket Cache is designed as a file-based alternative to in-memory caches like Redis and Memcached, and may not handle the demands of a busy store. For WooCommerce sites with heavy traffic, we recommend using Redis instead.

## I have a VPS. Can I use Docket Cache instead of Redis?

You can, but if your VPS supports Redis, we recommend using Redis for better performance. Docket Cache is best suited for environments where Redis or Memcached is not available. That said, Docket Cache works well for small to medium sites on a VPS, with no network connection required and no risk of cache-key conflicts.

## How do I know Docket Cache is working?

Open **Docket Cache → Overview** in your WordPress admin. The **Object Cache** line shows the size and number of cache files. If the numbers go up while you browse your site, Docket Cache is working.

If nothing is cached, check these:

1. The file `wp-content/object-cache.php` must exist. The Overview screen shows this under **Drop-in File**.
2. If the file `wp-content/.object-cache-delay.txt` exists, delete it. It is normally removed by itself a few seconds after you activate the plugin.
3. If you see "The Object Cache feature has been disabled at runtime", the [DOCKET\_CACHE\_DISABLED](constants.md#docket\_cache\_disabled) constant is set to `true` in your `wp-config.php` file. Remove it.

Sometimes the Overview screen shows "No Object Cache Files" for a short time on a site with many cache files. The numbers come back by themselves within a few minutes.

## Do I need OPcache to use Docket Cache?

No, but we strongly recommend it. Docket Cache saves the cache as PHP files, so it works with or without OPcache. OPcache keeps those files in memory, which makes them faster to read. Without it, the cache is read from disk each time.

If the Overview screen says OPcache is "Not Available" but your hosting company says it is turned on, your host has most likely blocked the PHP functions that Docket Cache uses to check it. This is common on shared hosting. Ask your host if any of these are disabled:

* `opcache_get_status`
* `opcache_get_configuration`
* `opcache_compile_file`
* `opcache_invalidate`
* `opcache_is_script_cached`
* `opcache_reset`

The same message appears when the host has set the `opcache.restrict_api` option. In both cases you can keep using Docket Cache.

## Can I stop a page from being cached?

No. Docket Cache is an object cache, not a page cache. It stores small pieces of data that WordPress uses to build a page, not the page itself, so it has no list of pages to skip. It also works the same way for visitors and for logged-in users.

What you can do is skip a cache group or a cache key:

1. Turn on **Cache Log** on the Configuration screen.
2. Open the page that shows old data, then look at the log. Each line shows the group and the key, like `"site-transient:update_core"`. Here `site-transient` is the group and `update_core` is the key.
3. Add the group or key to your `wp-config.php` file.

```php
define('DOCKET_CACHE_IGNORED_GROUPS', ['site-transient']);
```

See [DOCKET\_CACHE\_IGNORED\_GROUPS](constants.md#docket\_cache\_ignored\_groups), [DOCKET\_CACHE\_IGNORED\_KEYS](constants.md#docket\_cache\_ignored\_keys) and [DOCKET\_CACHE\_IGNORED\_GROUPKEY](constants.md#docket\_cache\_ignored\_groupkey) for details.

## How do I clear the cache from my own code?

When you add, edit or delete a post in the normal way, WordPress updates the cache for you. You only need to clear it yourself when data changes outside WordPress, for example after a bulk import or a direct change in the database.

Call the standard WordPress function:

```php
wp_cache_flush();
```

Or use WP-CLI:

```shell
wp cache flush
```

To clear the cache every time a post is saved, save this code as `wp-content/mu-plugins/docketcache_flush_when_save_post.php`:

```php
<?php
add_action('docketcache/init', function($docket_cache) {
    add_action('save_post', function($post_id, $post, $update) use($docket_cache) {
        $result = $docket_cache->flush_cache(true);
        $docket_cache->co()->lookup_set('occacheflushed', $result);
        do_action('docketcache/action/flush/objectcache', $result);
    }, 10, 3);
});
```

To clear the cache every hour, save this code as `wp-content/mu-plugins/docketcache_flush_every_hour.php`:

```php
<?php
add_action('docketcache/init', function($docket_cache) {
    add_action('docketcache_flush_every_hour', function() use($docket_cache) {
        $result = $docket_cache->flush_cache(true);
        $docket_cache->co()->lookup_set('occacheflushed', $result);
        do_action('docketcache/action/flush/objectcache', $result);
    });

    if (!wp_next_scheduled('docketcache_flush_every_hour')) {
        wp_schedule_event(time(), 'hourly', 'docketcache_flush_every_hour');
    }
});
```

Files in `mu-plugins` are not changed when Docket Cache is updated.

## Why do I get a fatal error about unserialize()?

The error looks like this:

```
Fatal error: Uncaught TypeError: unserialize(): Argument #1 ($data) must be of type string
```

It comes from another plugin, not from Docket Cache. Docket Cache gives the data back already unpacked, and the other plugin tries to unpack it a second time with `unserialize()`.

There are two ways to fix it:

1. Ask the author of that plugin to use the WordPress function `maybe_unserialize()` in place of `unserialize()`.
2. Until then, turn on **Retain Transients in Db** on the Configuration screen. Docket Cache then leaves transients in the database and does not cache them. See [DOCKET\_CACHE\_TRANSIENTDB](constants.md#docket\_cache\_transientdb).

For any other error, turn on WordPress debugging to find out which plugin causes it:

```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
```

## How do I remove Docket Cache completely?

1. Go to the **Plugins** screen and click **Deactivate** under Docket Cache.
2. Click **Delete**.

Then check that these are gone from your server, and delete any that are left:

* `wp-content/object-cache.php`
* `wp-content/cache/docket-cache`
* `wp-content/docket-cache-data`
* `wp-content/plugins/docket-cache`

If your site shows an error and you cannot open the WordPress admin, delete the same files and folders with FTP or your hosting file manager.

## My security scanner reports files from Docket Cache. Is that a problem?

Docket Cache saves its cache and its own settings as PHP files, not in the database. They are kept in two folders:

* `wp-content/cache/docket-cache` for the cache files.
* `wp-content/docket-cache-data` for settings and statistics, for example `cachestats.php`.

Some scanners flag these files only because they are PHP files that change often. A normal Docket Cache file does nothing except return a list of data.

If a scanner reports a file that holds other code, it does not come from Docket Cache. Delete the `wp-content/cache/docket-cache` folder, which is safe because the cache is built again, and contact the maker of your scanner.

## Can I use Docket Cache on more than one server?

Docket Cache was built and tested for one server with a local disk.

* **Several servers sharing one database:** each server keeps its own cache files, so they can go out of step. We do not recommend Docket Cache for this setup.
* **Network storage such as NFS or Amazon EFS:** reading and writing cache files over the network is slower than a local disk, so we do not recommend it either.
* **WordPress Multi-Network:** this is supported. Each sub-network gets its own cache folder. See [DOCKET\_CACHE\_PATH\_NETWORK\_(n)](constants.md#docket\_cache\_path\_network\_n) to change the folder for a sub-network.
