---
description: How disk I/O, IOPS, inode and process limits work on shared hosting, what happens when a WordPress site reaches them, and how to keep Docket Cache within them.
---

# Web Hosting I/O Usage

Shared hosting plans advertise disk space and bandwidth, but the limits that decide how a WordPress site behaves under load are usually the ones in the small print: how fast the account may read and write to disk, how many files it may hold, and how many processes it may run at once. Because Docket Cache stores the object cache as files, we think it is worth explaining these limits plainly, including what our plugin adds to them and how to keep that bounded.

## Three different disk limits

Disk limits are often lumped together as "I/O", but there are three separate things being measured.

| Limit | What it measures | Typical unit |
| --- | --- | --- |
| I/O throughput | The amount of data read and written per second, combined | KB/s or MB/s |
| IOPS | The number of read and write operations per second, whatever their size | operations per second |
| Inodes | The number of files and directories the account owns | count |

Throughput and IOPS are rate limits: they cap how hard the account can work the disk at any moment. A backup that writes one large archive is heavy on throughput, while a job that touches thousands of small files is heavy on IOPS.

The inode limit is a quota, not a rate. Every file and every directory uses one inode regardless of its size, so an account can run out of inodes while most of its disk space is still free.

## How the limits are enforced

Most cPanel shared hosts run CloudLinux, which places each account in its own container called an LVE (Lightweight Virtual Environment) and applies a set of limits to it. The values are chosen by the hosting provider and differ from plan to plan.

| Limit | Meaning | When it is reached |
| --- | --- | --- |
| SPEED | CPU share, relative to one core | Processes are slowed down |
| PMEM | Physical memory, including shared memory and disk cache | Disk cache is released first, then processes are killed, which usually shows as a 500 or 503 error |
| IO | Read and write throughput | Processes are put to sleep until they are back under the limit |
| IOPS | Read and write operations per second | Further operations wait until the current second ends |
| EP | Entry processes: concurrent requests to PHP and other dynamic scripts, plus SSH sessions and cron jobs | The web server returns a 508 "Resource Limit Reached" error |
| NPROC | Total number of processes in the account | No new process can be created, which usually shows as a 500 or 503 error |

The important distinction is that the two disk limits throttle and do not fail. A site that reaches its IO or IOPS limit keeps working, only slowly. Nothing is written to the PHP error log, so the cause is easy to miss.

Slow requests then create a second problem. Each PHP request holds an entry process for as long as it runs, so when requests are stalled waiting for the disk they overlap, the EP limit fills up, and visitors start receiving 508 errors. An account that shows EP faults is quite often an account with a disk or CPU bottleneck underneath.

Inodes are enforced separately through the disk quota. Once the hard limit is reached, the account can no longer create files, so uploads, updates, sessions and cache writes all fail.

{% hint style="info" %}
On CloudLinux, the IO limit does not count data served from the operating system's disk cache. Files that are read repeatedly cost far less than files that are constantly rewritten.
{% endhint %}

## Reading the numbers in cPanel

CloudLinux adds a Resource Usage page to the Metrics section of cPanel, where the host has enabled it. It has three parts:

- **Dashboard** reports whether the account has been limited in the last 24 hours and which resource was responsible.
- **Current Usage** charts each limit over time with its usage, its limit and its faults. A fault is recorded each time a limit is hit.
- **Snapshot** lists the processes, database queries and HTTP requests that were running when a limit was hit.

Inode usage appears on the same page when the host has enabled inode limits.

Start with the faults column. Faults on IO or IOPS that line up with a backup or a scheduled task point to that job. Faults on EP with no matching traffic peak suggest that requests are slow, not numerous, and the snapshot will usually show which URL or query is responsible.

## What drives disk I/O in WordPress

In our experience the heavy disk users are rarely ordinary page views. They are background and maintenance jobs:

- Backup plugins that archive the whole site into the same account.
- Malware and integrity scanners that read every file.
- Bulk imports, image regeneration and thumbnail generation.
- Page cache or minification plugins that purge and rebuild their whole cache after each change.
- Debug and access logging left switched on, including `WP_DEBUG_LOG`.
- Bots and crawlers requesting large numbers of uncached URLs.
- Scheduled tasks that all fire in the same minute.

For inodes the usual causes are different: mail left on the server, old backups, file-based sessions, multiple image sizes for every upload, and cache directories that are never pruned.

## Where Docket Cache fits

We want to be straightforward about this. Docket Cache is a file-based object cache. Each cached object is written to disk as a small PHP file, so the plugin uses both write I/O and inodes. A Redis or Memcached cache does not, but those services are rarely offered on shared plans, which is the gap Docket Cache exists to fill.

The read side is where the design pays off. Cache files are plain PHP code, so OPcache compiles them once and keeps the compiled result in shared memory. Later requests use that copy and PHP does not have to read and parse the file again. Disk activity is therefore concentrated on writes: when a cache entry is first created, when it changes, and when the Garbage Collector cleans up. The Garbage Collector is a Cron Event that runs every five minutes.

By default the cache lives in `wp-content/cache/docket-cache`. You can check how much of your quota it uses from a shell:

```shell
find wp-content/cache/docket-cache -type f | wc -l
du -sh wp-content/cache/docket-cache
```

## Keeping the cache within your limits

These constants bound what Docket Cache may use. They are defined in `wp-config.php`.

| Constant | Default | Effect |
| --- | --- | --- |
| [DOCKET_CACHE_MAXFILE](../constants.md#docket_cache_maxfile) | 50000 | Maximum number of cache files on disk |
| [DOCKET_CACHE_MAXSIZE_DISK](../constants.md#docket_cache_maxsize_disk) | 524288000 (500MB) | Maximum size of the cache storage on disk |
| [DOCKET_CACHE_MAXSIZE](../constants.md#docket_cache_maxsize) | 3145728 (3MB) | Maximum size of the object data stored in one cache file |
| [DOCKET_CACHE_PRECACHE_MAXFILE](../constants.md#docket_cache_precache_maxfile) | 100 | Maximum number of precache files on disk |

On a plan with a tight inode limit, lower the file ceiling so that the cache cannot take more than the share you are willing to give it:

```php
define('DOCKET_CACHE_MAXFILE', 20000);
define('DOCKET_CACHE_MAXSIZE_DISK', 104857600);
```

`DOCKET_CACHE_MAXFILE` accepts values between 200 and 1000000, and `DOCKET_CACHE_MAXSIZE_DISK` has a minimum of 104857600 bytes (100MB).

Several other constants are useful when files, not bytes, are the problem:

- [DOCKET_CACHE_MAXFILE_LIVECHECK](../constants.md#docket_cache_maxfile_livecheck) monitors the file limit in real time.
- [DOCKET_CACHE_FLUSH_DELETE](../constants.md#docket_cache_flush_delete) deletes an expired cache file. By default the file is only emptied, so it still occupies an inode.
- [DOCKET_CACHE_EMPTYCACHE_IGNORE](../constants.md#docket_cache_emptycache_ignore) stops empty caches from being stored on disk.
- [DOCKET_CACHE_STALECACHE_IGNORE](../constants.md#docket_cache_stalecache_ignore) stops stale cache left behind by WordPress, WooCommerce and others from being stored on disk.
- [DOCKET_CACHE_FLUSH_STALECACHE](../constants.md#docket_cache_flush_stalecache) lets the Garbage Collector remove that stale cache immediately after cache invalidation.
- [DOCKET_CACHE_IGNORED_GROUPS](../constants.md#docket_cache_ignored_groups) excludes whole cache groups, which helps when one plugin generates a very large number of entries.

All of these are off by default, apart from the ignored groups list which has its own defaults. We suggest enabling the two `_IGNORE` constants only if you actually have an inode problem.

Two more settings affect disk behaviour without reducing usage. [DOCKET_CACHE_CHUNKCACHEDIR](../constants.md#docket_cache_chunkcachedir) splits the cache into smaller directories, which helps when one very large directory has become slow to list or clear; the number of files stays the same. [DOCKET_CACHE_PATH](../constants.md#docket_cache_path) moves the cache directory. On a server you control, pointing it at a RAM disk removes cache writes from the disk entirely, but that needs root access and is not an option on shared hosting.

Finally, keep [DOCKET_CACHE_LOG](../constants.md#docket_cache_log) switched off unless you are debugging, since the cache log is one more file being written to disk.

## A practical order of work

1. Open the Resource Usage page and note which limit records faults and at what time.
2. Match the time to a job: a backup, a scan, an import or a scheduled task. Move it to a quiet hour, or send backups to remote storage.
3. Turn off debug logging and remove old backups, logs and unused cache directories.
4. Count the files in your cache directories and set `DOCKET_CACHE_MAXFILE` to suit your inode allowance.
5. If faults continue with nothing left to trim, the site has outgrown the plan. Ask your host for the actual figures of the next tier before upgrading.
