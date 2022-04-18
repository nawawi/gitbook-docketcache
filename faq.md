---
description: Frequently Asked Questions
---

# FAQ

## What is Object Caching in WordPress?

Object caching is a process that stores database query results in order to quickly bring them back up next time they are needed.

The cached object will be served promptly from the cache rather than sending multiple requests to a database. This is more efficient and reduces massive unnecessary loads on your server.

In simple terms, object caching allows objects that are used often to be copied and stored at a closer location for quicker use.

## What is Docket Cache in Object Caching?

By default, the object cache in WordPress is non-persistent. This means that data stored in the cache reside in memory only and only for the duration of the request. Cached data will not be stored persistently across page loads. To make it persistent, the object cache must be stored on a local disk.

Docket Cache is not just stored the object cache, it converts the object cache into plain PHP code. This solution is faster since WordPress can use the cache directly without running other operation.

## What is the Cronbot Service in Docket Cache?

The Cronbot is an external service that pings your website every hour to keep WordPress Cron running actively.

This service offered as an alternative option and is not compulsory to use. By default, this service not connected to the [end-point server](https://cronbot.docketcache.com). You can completely disable it at the configuration page.

## What is Garbage Collector in Docket Cache?

Garbage Collector is a Cron Events than run every 5 minutes to monitoring cache file purposely for cleanup and collecting stats.

## What is OPcache in Docket Cache?

OPcache is a caching engine built into PHP, that improves performance by storing precompiled script bytecode in shared memory, thereby removing the need for PHP to load and parse scripts on each request.

Docket Cache converts the object cache into plain PHP code. When reading and writing cache, it will use OPcache directly which results in faster data retrieval and better performance.

## What is a RAM disk in Docket Cache?

A RAM disk is a representation of a hard disk using RAM resources, and it can take the form of a hardware device or a virtual disk.

Read and write speed on RAM is multiple times faster than SSD drives therefore storing Docket Cache files on a RAM disk greatly increases it's performance.

Do note that creating RAM disks requires server administrative permission (root access) so this solution is not suitable for shared hosting servers.

This is an example command to create and use a RAM disk with Docket Cache:

```
$ cd wp-content/
$ sudo mount -t tmpfs -o size=500m tmpfs ./cache/docket-cache
```

Kindly refer to the articles below about RAM disk:

1. [How to Easily Create RAM Disk](https://www.linuxbabe.com/command-line/create-ramdisk-linux)
2. [What Is /dev/shm And Its Practical Usage](https://www.cyberciti.biz/tips/what-is-devshm-and-its-practical-usage.html)
3. [Creating A Filesystem In RAM](https://www.cyberciti.biz/faq/howto-create-linux-ram-disk-filesystem/)

To use it in Windows OS, create RAM Disk and change [DOCKET\_CACHE\_PATH](https://docs.docketcache.com/configuration#docket\_cache\_path) point to RAM Disk drive.

## What is the minimum RAM required to use with shared hosting?

By default, WordPress allocates the memory limit to 256 MB. Combined with MySQL and Web Server, you need more than 256 MB. If you're using a cheap hosting plan that allocates only 256 MB for totals usage. It is not enough, and Docket Cache can't improve your website performance.

## What’s the difference with the other object cache plugin?

Docket Cache is an Object Cache Accelerator. It does some optimisation like cache post queries, comments counting, WordPress translation and more before storing the object caches.

## Can I pair using it with other cache plugins?

Yes and No. You can pair using it with page caching plugin, but not with the object cache plugin.

## I'm using a VPS server. Can I use Docket Cache to replace Redis?

Yes, you can. It can boost more your WordPress performance since there is no network connection need to makes and no worry about memory burst, cache-key conflict and error-prone caused by the improper settings.
