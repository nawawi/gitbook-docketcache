---
description: How the main types of WordPress hosting differ, what limits performance on each, and what to check before choosing a host.
---

# WordPress Hosting

Hosting plans that look alike on a pricing page can behave very differently under load. In this article we look at hosting the way WordPress experiences it: how much CPU, memory, disk and PHP capacity a site really gets, who is responsible for the server, and which caching options are open to you.

## What WordPress Needs From a Server

A page that is not served from a page cache goes through the same steps on every request: the web server hands the request to PHP, PHP loads WordPress core, the theme and every active plugin, WordPress runs its database queries, and the result is sent back. Four resources decide how quickly that happens.

- **CPU** executes PHP and the database queries. It is the first thing to run short on a busy dynamic site such as a shop, a membership site or a forum.
- **Memory** is shared between PHP processes, the database server, the web server and any caches. When it runs out, processes are killed or the server starts swapping.
- **Disk** matters for more than storage space. Read and write speed affects the database and every file-based cache, and the number of files you may keep is often limited separately from the space they occupy.
- **PHP concurrency** is the number of requests PHP can work on at the same moment. Once every worker is busy, new requests wait in a queue, and visitors see a slow site or an error even though the server looks idle by other measures.

Caching exists to reduce how often these resources are needed. A page cache skips PHP altogether for anonymous visitors. OPcache keeps compiled PHP scripts in shared memory, so they are not parsed again on every request. A persistent object cache keeps the results of database queries between requests. How much of this is available depends on the type of hosting.

## Types of Hosting

### Shared Hosting

Many accounts live on one server and share its CPU, memory and disk. The host manages everything and gives you a control panel. To stop one account from starving the others, each account is placed inside resource limits: a share of CPU, a memory ceiling, a cap on concurrent processes (often called entry processes), disk I/O throttling and a maximum number of files (inodes).

Those limits, not the hardware, are what a WordPress site runs into: a site can be slow simply because the account has reached its own ceiling. Redis and Memcached are rarely offered on shared plans, and you cannot install them yourself.

### VPS

A virtual private server gives you a fixed allocation of CPU cores, memory and disk on a shared physical machine, with root access. Nobody imposes per-account limits, because the whole virtual machine is yours. The limit is the size of the VPS and how well it is configured.

An unmanaged VPS, such as one from [DigitalOcean](https://m.do.co/c/6c93db5b1ef6), leaves the operating system, web server, PHP, database, security updates and backups to you. In return you choose the PHP version, size OPcache yourself and can install any service you like. A managed VPS adds a provider who maintains the stack for you, usually in exchange for less freedom over what may be installed.

Memory is the usual constraint on a small VPS. The database, PHP workers and any in-memory cache all draw from the same pool, so every extra service has to be budgeted for.

### Cloud Hosting

Cloud platforms sell virtual machines, storage and managed databases that can be resized or multiplied on demand, with billing based on usage. For a single WordPress site, a cloud instance behaves much like a VPS.

Two details deserve attention. Some instance types offer burstable CPU, which is fast for short peaks but throttled under sustained load. Storage is often network-attached, with its own performance allowance, so disk speed is something you select rather than assume.

### Managed WordPress Hosting

Here the provider runs a stack built specifically for WordPress and takes care of updates, backups, security and server-level page caching. Staging environments and a CDN are commonly part of the service.

Capacity on these plans is typically expressed in PHP workers and visits rather than CPU cores. Sites that are mostly cached pages need few workers. Sites where most requests cannot be cached, such as shops with baskets and logged-in users, need many more. The trade-off for the convenience is control: some plugins are disallowed, server settings are fixed, and an in-memory object cache may be included, sold as an add-on or absent.

### Dedicated Servers

A dedicated server is a physical machine used by nobody else. There is no contention with other customers for CPU, memory or disk, and performance is predictable. The constraints are cost, the effort of administration unless you pay for management, and the fact that upgrading means moving to different hardware.

### Summary

| Type | Server managed by | Usual first bottleneck | Redis/Memcached |
| --- | --- | --- | --- |
| Shared | Host | Per-account limits: CPU, entry processes, I/O, inodes | Rarely offered |
| VPS | You, or a managed provider | Memory and configuration | Install it yourself |
| Cloud | You, or a managed provider | CPU throttling on burstable instances, storage performance | Self-installed or a separate service |
| Managed WordPress | Host | PHP workers | Depends on the plan |
| Dedicated | You, or a managed provider | Hardware size | Install it yourself |

## PHP Version, Handler and OPcache

The PHP setup has as much influence on WordPress as the hardware does.

**PHP version.** Each major PHP release has been faster than the previous one, and versions that are no longer supported stop receiving security fixes. A host should offer currently supported versions and let you switch between them per site.

**PHP handler.** The handler is how the web server runs PHP. PHP-FPM, used with Nginx and Apache, and LSAPI, used with LiteSpeed, both keep PHP processes alive between requests. This is what allows OPcache to keep compiled scripts in shared memory. Older CGI-style handlers start a fresh PHP process for each request, which is slower and leaves OPcache with nothing to reuse between requests.

**OPcache.** Check that OPcache is enabled and how much memory it has been given. WordPress core, a theme and a typical set of plugins amount to thousands of PHP files, and when OPcache memory is full, scripts are no longer cached. The settings that matter most are `opcache.memory_consumption` and `opcache.max_accelerated_files`. On shared hosting these are set by the host; on a VPS they are yours to tune. Our "OPcache Extension" article explains how the extension works.

On a server with shell access, the following shows the current settings and the inode usage of the account:

```bash
php -i | grep -E "opcache\.(enable|memory_consumption|max_accelerated_files)"
df -i
```

{% hint style="info" %}
The command-line PHP can use a different configuration from the one that serves your website. If in doubt, check the PHP information page in your hosting control panel, or the Site Health screen in WordPress.
{% endhint %}

## Where Docket Cache Fits

WordPress has an object cache built in, but by default it lasts only for the duration of a single request. Making it persistent normally means running Redis or Memcached, which is exactly what low-cost and shared hosting tends not to provide.

We built Docket Cache for that situation. It stores the object cache as plain PHP files, so OPcache can compile them and serve them from shared memory. There is no extra daemon to install, no network connection to a cache server and nothing to ask the host for. The requirements are PHP 7.2.5 or later and WordPress 5.4 or later, with Zend OPcache recommended.

What it asks of the hosting follows from that design:

- **OPcache should be enabled, with memory to spare.** Docket Cache works without OPcache, but OPcache is what makes its cache files fast to read. Cache files are PHP scripts, so they share OPcache memory with WordPress itself.
- **Reasonable disk I/O.** Cache files are written to disk, so heavily throttled storage reduces the benefit. Our "Web Hosting I/O Usage" article covers I/O, IOPS and entry process limits in detail.
- **Inode headroom.** Each cached object is a file. By default Docket Cache keeps at most 50,000 cache files and 500MB on disk, and both limits can be lowered with `DOCKET_CACHE_MAXFILE` and `DOCKET_CACHE_MAXSIZE_DISK` if your plan has a tight file quota.
- **Enough memory overall.** An account limited to 256MB in total for PHP, the database and the web server is too small for WordPress to perform well, and no cache plugin can change that.

On a VPS or dedicated server where Redis is available, we recommend Redis for better performance, particularly for a busy WooCommerce store. Docket Cache still works well there for small to medium sites: it removes one service from the memory budget, and with root access the cache directory can be mounted on a RAM disk for faster reads and writes. On LiteSpeed servers it works alongside the LiteSpeed Cache plugin, provided that plugin's own Object Cache option is turned off.

## What to Check Before Choosing a Host

Prices and plan names change constantly, so compare hosts on what they commit to in writing.

1. Which PHP versions are offered, and can you switch version yourself?
2. Which PHP handler is used, and is OPcache enabled? How much memory does it have?
3. What are the account limits for CPU, memory, entry processes, disk I/O and inodes? A plan described as unlimited still has them, usually in the acceptable use policy.
4. How many PHP workers does the plan include, if it is sold that way?
5. Is the storage SSD or NVMe?
6. Is Redis or Memcached available, and at what cost? If not, a file-based object cache such as Docket Cache fills the gap.
7. Do you get SSH and WP-CLI access?
8. How are backups taken, and is there a staging environment?
9. Where are the data centres in relation to your visitors?
10. What happens when a limit is reached: is the site throttled, suspended or billed extra?

## Tuning the Hosting You Already Have

Moving host is not always necessary. Work through the cheaper options first.

- Switch to the newest PHP version that your theme and plugins support.
- Confirm that OPcache is active, and ask the host to raise its memory if it is full.
- Enable a page cache, so anonymous visitors do not consume PHP workers.
- Add a persistent object cache, to cut repeated database queries for logged-in users and dynamic pages.
- Remove plugins you do not use. Each one adds PHP files to load and often database queries on every request.
- Look at the resource usage graphs in your control panel. They show which limit you are reaching, and so whether a larger plan or a different type of hosting is needed.
