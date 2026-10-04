---
description: How PHP's OPcache extension works, why Docket Cache is faster with it, and which settings to review on PHP 8.x.
---

# OPcache Extension

OPcache is the opcode cache that ships with PHP. Docket Cache works without it, but we strongly recommend it, because the design of the plugin is built around it: we store cached objects as PHP files so that OPcache can keep them in memory. This article explains what the extension does, how it uses memory, and which `php.ini` directives deserve attention on a WordPress site running PHP 8.x.

## What OPcache does

PHP does not execute source code directly. Every script goes through the same pipeline before a single line runs:

| Stage | What happens | With OPcache |
| --- | --- | --- |
| Read | The file is read from disk. | Skipped on a cache hit. |
| Parse | The source is tokenised and turned into a syntax tree. | Skipped on a cache hit. |
| Compile | The tree is converted into opcodes, the instructions the Zend Engine understands. | Skipped on a cache hit. |
| Execute | The engine runs the opcodes. | Always happens. |

Without an opcode cache, the first three stages are repeated for every file on every request, and the result is thrown away when the request ends. A WordPress page load includes hundreds of files from core, the theme and the active plugins, so that repeated work adds up quickly.

OPcache removes it. The first time a script is compiled, the opcodes are copied into a block of shared memory that every PHP worker process can read. Later requests, from any worker, fetch the compiled script from memory and go straight to execution. Before storing a script, OPcache also runs it through an optimiser that simplifies the opcodes, for example by evaluating constant expressions ahead of time and removing code that can never run.

Two consequences are worth remembering:

- The cache is shared between workers, but it lives in memory only. Restarting PHP-FPM or the web server empties it.
- OPcache has to decide when a cached script is out of date. How it does that is configurable, and it matters a great deal for a plugin that writes PHP files at runtime.

## Why it matters for Docket Cache

Most file-based object caches write each object to disk with `serialize()` and rebuild it with `unserialize()` on every request. That works, but the file has to be read and decoded each time it is needed.

Docket Cache writes the object as PHP code instead. A simplified illustration of the idea, not the exact file format:

```php
<?php
return [
    'group' => 'options',
    'key'   => 'alloptions',
    'data'  => [
        'blogname'       => 'Example Site',
        'posts_per_page' => 10,
    ],
];
```

To PHP, this is an ordinary script. Once OPcache has compiled it, the array is held in shared memory, and including the file again costs no disk read and no decoding step. That is how the plugin gets in-memory behaviour without a Redis or Memcached server.

It also means the plugin and your WordPress code compete for the same OPcache resources. A site with tens of thousands of cache files needs a larger opcode cache than the PHP defaults provide, which is what the rest of this article is about.

## Checking that OPcache is active

From a shell, you can confirm the extension is loaded and see its settings:

```shell
php -v
php --ri "Zend OPcache"
```

`php -v` prints a line mentioning Zend OPcache when the extension is loaded. On builds where it is packaged separately, install your distribution's OPcache package, or load it in `php.ini` with `zend_extension=opcache`.

{% hint style="info" %}
The command line and the web server usually read different configuration files, and `opcache.enable_cli` is off by default. A CLI process also has its own separate cache. Treat shell output as a hint, and check the values the web server actually uses, for example on the Docket Cache Overview screen or with `phpinfo()`.
{% endhint %}

## How OPcache uses memory

OPcache reserves one shared memory segment when PHP starts. The segment does not grow afterwards, so three limits set at start-up decide how much it can hold.

| Limit | Directive | Default | What it bounds |
| --- | --- | --- | --- |
| Total size | `opcache.memory_consumption` | 128 (MB) | Everything OPcache stores. |
| Strings | `opcache.interned_strings_buffer` | 8 (MB) | Shared copies of identical strings. |
| Entries | `opcache.max_accelerated_files` | 10000 | Keys in the script hash table. |

### Total size

Compiled scripts, the interned strings buffer and the hash table all live inside the `opcache.memory_consumption` segment. When it is full, new scripts cannot be cached and are compiled on every request, exactly as if OPcache were not there.

### Interned strings

PHP stores each distinct string literal, function name, class name and array key once and reuses it. OPcache moves that store into shared memory so that all workers share one copy instead of each holding their own. Cache files are full of repeated array keys and option names, so this buffer fills faster on a site running Docket Cache than on a plain WordPress install. If it runs out, PHP keeps working, but new strings are held in each worker's private memory and the saving is lost.

### Entries

`opcache.max_accelerated_files` is the size of the hash table used to look up cached scripts. PHP does not use the number as given. It rounds up to the next value in a fixed list of primes: 223, 463, 983, 1979, 3907, 7963, 16229, 32531, 65407, 130987, 262237, 524521, 1048793. The default of 10000 therefore gives 16229 slots. Values below 200 or above 1000000 are clamped.

The limit counts keys, not files, and a single file can be registered under more than one key. This is why `num_cached_keys` in the status output can be higher than `num_cached_scripts`.

### Wasted memory and restarts

OPcache does not reclaim memory piece by piece. When a cached script is replaced or invalidated, its old copy stays in the segment and is counted as wasted. Nothing is freed until the whole cache is restarted.

A restart is scheduled when OPcache runs short of free memory and the wasted share exceeds `opcache.max_wasted_percentage` (default 5, maximum 50). A restart is also triggered when the hash table runs out of slots, and it can be requested manually with `opcache_reset()`. After a restart the cache is empty and every worker has to compile its scripts again, so requests are slower until it is warm.

This matters for Docket Cache because cache files change far more often than application code. Every changed file leaves a wasted copy behind. On a busy site with an undersized segment, the result is a cycle of fill, restart and refill. Giving OPcache enough room is the fix.

## Keeping cached scripts fresh

Three directives control how OPcache notices that a file has changed.

| Directive | Default | Effect |
| --- | --- | --- |
| `opcache.validate_timestamps` | 1 | Compare the file's modification time with the cached copy. |
| `opcache.revalidate_freq` | 2 | Seconds between those checks for a given script. `0` checks on every request. |
| `opcache.file_update_protection` | 2 | Do not cache files modified less than this many seconds ago. |

With `opcache.validate_timestamps=0`, OPcache never looks at the file again. Changes take effect only after `opcache_invalidate()`, `opcache_reset()` or a PHP restart. This is slightly faster and is popular for applications deployed as immutable releases.

WordPress is not that kind of application. Plugins, themes and core are updated in place from the dashboard, and files are often edited over SFTP. For most sites we recommend leaving timestamp validation on. Docket Cache does not rely on the timestamp check for its own files: it writes each cache file atomically, asks OPcache to compile it, and invalidates the entry when the file is changed or removed. Turning validation off is therefore possible, but you then take responsibility for resetting OPcache whenever any other PHP file on the site changes outside the WordPress updater.

`opcache.file_update_protection` exists to stop OPcache caching a file that is still being written. Leave it at the default unless you are certain every writer on the server replaces files atomically.

## Settings to review

The defaults are sized for a small application. The following is a reasonable starting point for a WordPress site using Docket Cache with its default limits. Treat the numbers as a first estimate and adjust them from what monitoring shows.

```ini
opcache.enable=1
opcache.memory_consumption=256
opcache.interned_strings_buffer=16
opcache.max_accelerated_files=65407
opcache.max_wasted_percentage=10
opcache.validate_timestamps=1
opcache.revalidate_freq=2
```

These directives are read when PHP starts, so restart PHP-FPM or the web server after changing them. On shared hosting you may not be able to change them at all. In that case, fit Docket Cache to what the host provides instead.

### Matching the entry limit to the cache size

Docket Cache stores up to `DOCKET_CACHE_MAXFILE` cache files, 50000 by default. With the default `opcache.max_accelerated_files`, OPcache has 16229 slots for the cache files and all of WordPress together. Either raise the OPcache limit so it covers both, as in the example above, or lower the plugin's limit in `wp-config.php`:

```php
define('DOCKET_CACHE_MAXFILE', 10000);
```

### Directives that can disable caching for the plugin

A few settings quietly prevent Docket Cache files from reaching shared memory:

- `opcache.blacklist_filename` points to a list of paths that are never cached. If the WordPress directory or the cache directory matches an entry, cache files are compiled on every request.
- `opcache.file_cache_only=1` stores opcodes in files on disk and does not use shared memory, which removes the benefit the plugin is built around.
- `opcache.max_file_size` skips files larger than the given number of bytes. The default of `0` caches everything.
- `opcache.restrict_api` limits which scripts may call the OPcache functions. When it excludes your site, or when the functions are listed in `disable_functions`, the plugin cannot read OPcache statistics or flush it.

### Directives we leave alone

`opcache.save_comments` should stay on, because many plugins and libraries read docblock comments at runtime. `opcache.enable_file_override` can return stale answers from `file_exists()` when timestamp validation is off, and gains little on WordPress. `opcache.revalidate_path` and `opcache.use_cwd` are fine at their defaults.

## Monitoring

`opcache_get_status()` reports how full the cache is. Run this through the web server rather than the command line, so that it reads the cache your site uses:

```php
<?php
$status = function_exists('opcache_get_status') ? opcache_get_status(false) : false;

if (false === $status) {
    exit("OPcache status is not available.\n");
}

$memory = $status['memory_usage'];
$stats  = $status['opcache_statistics'];
$mb     = 1024 * 1024;

printf("Used memory:   %.1f MB\n", $memory['used_memory'] / $mb);
printf("Free memory:   %.1f MB\n", $memory['free_memory'] / $mb);
printf("Wasted:        %.1f%%\n", $memory['current_wasted_percentage']);
printf("Keys:          %d of %d\n", $stats['num_cached_keys'], $stats['max_cached_keys']);
printf("Hit rate:      %.2f%%\n", $stats['opcache_hit_rate']);
printf("OOM restarts:  %d\n", $stats['oom_restarts']);
printf("Hash restarts: %d\n", $stats['hash_restarts']);
```

Remove the script once you have the numbers. What to look for:

| Reading | Meaning | Action |
| --- | --- | --- |
| Free memory near zero, or `cache_full` is true | The segment is too small. | Raise `opcache.memory_consumption`. |
| `oom_restarts` keeps rising | OPcache is restarting because it ran out of memory. | Raise `opcache.memory_consumption`. |
| Keys close to the maximum, or `hash_restarts` rising | The hash table is too small. | Raise `opcache.max_accelerated_files` or lower `DOCKET_CACHE_MAXFILE`. |
| Interned strings `free_memory` near zero | The strings buffer is full. | Raise `opcache.interned_strings_buffer`. |
| Low hit rate on a site that has been up for a while | Scripts are being recompiled. | Check the three readings above and the blacklist. |

You do not need a separate tool for this. The Docket Cache Overview screen shows how much OPcache memory is used by cache files and by WordPress files, and the OPcache Viewer, enabled on the Configuration screen or with `DOCKET_CACHE_OPCVIEWER`, lists the cached scripts and current usage.

For a closer look at why restarts happen, set `opcache.log_verbosity_level` to 2 or higher temporarily. Warnings are written to the file named in `opcache.error_log`, or to the web server error log when that is empty.

## Changes in recent PHP versions

Older tuning guides mention directives that no longer exist. If you are carrying a `php.ini` forward from an earlier PHP version, these can be deleted:

| Directive | Status |
| --- | --- |
| `opcache.fast_shutdown` | Removed in PHP 7.2. The behaviour is built into PHP. |
| `opcache.inherited_hack` | Removed in PHP 7.3. |
| `opcache.consistency_checks` | Disabled in PHP 8.1.18 and 8.2.5, removed in PHP 8.3. |

PHP 8.0 added a JIT compiler to OPcache, controlled by `opcache.jit` and `opcache.jit_buffer_size`. It is disabled by default as of PHP 8.4, and when enabled its buffer is allocated in addition to `opcache.memory_consumption`. Docket Cache does not need it. A typical WordPress request spends its time on database queries and I/O rather than on the CPU-bound loops the JIT is good at, so we suggest sizing the opcode cache properly first and treating the JIT as an optional experiment.

## Summary

- OPcache keeps compiled scripts in shared memory, and Docket Cache uses that to serve cached objects from memory rather than from disk.
- The segment is fixed in size. Memory, interned strings and the number of keys each have their own limit, and the defaults are small for a site with a large object cache.
- Size `opcache.max_accelerated_files` to cover WordPress and `DOCKET_CACHE_MAXFILE` together.
- Keep timestamp validation on unless you control every change to the site's PHP files.
- Watch free memory, the key count and the restart counters, and increase the limits before OPcache starts restarting itself.
