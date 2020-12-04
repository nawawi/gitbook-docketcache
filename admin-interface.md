---
description: Docket Cache WP Admin Interface
---

# Admin Interface

The Docket Cache keeps the admin interface clean, responsive and simple as possible, predefined configuration and reuses WordPress library as much as possible.

## Overview

The Overview section is a central place to view the current status of Docket Cache activities, configuration and other useful information.

| Label | Description |
| :--- | :--- |
| Web Server | Web Server name. |
| PHP SAPI | PHP version and type of Server API. |
| Cloudflare | Cloudflare IP and Ray ID. Only visible if your website running behind Cloudflare. |
| Web Proxy | Web Proxy IP. Only visible if behind web proxy other than Cloudflare, like varnish. |
| Object Cache Stats | Total object size in cache files. |
| Object OPcache Stats | Total OPcache size in memory for an object, compiled from cache files. |
| WP OPcache Stats | Total OPcache size in memory for your website, compiled from WordPress files. |
| PHP Memory Limit | Your Server PHP memory limit setting. |
| WP Frontend Memory Limit | WordPress Website memory limit. |
| WP Backend Memory Limit | WordPress Admin memory limit. |
| Drop-in Writable | Status either Drop-in file can be written, replace or delete. |
| Drop-in File | Drop-in file location. |
| Cache Writable | Status either cache file can be written, replace or delete. |
| Cache Files Limit | Current total cache files and maximum files can be store on disk. |
| Cache Disk Limit | Current total size cache files and maximum size can be store on disk. |
| Cache Path | Cache directory. |
| Config Writable | Status either config file can be written, replace or delete. |
| Config Path | Config location. |

## Configuration

The configuration section allows you to change the Docket Cache behaviour without using constant variables. If related constant defined in the `wp-config.php` file, it will overwrite the changes in this section.

| Option | Constant |
| :--- | :--- |
| Cronbot Service | [DOCKET\_CACHE\_CRONBOT](configuration.md#docket_cache_cronoptmzdb) |
| Cache Log |  |
| Advanced Post Caching |  |
| Object Cache Precaching |  |
| WordPress Translation Caching |  |
| Optimize WP Query |  |
| Optimize Term Count Queries |  |
| Optimize Database Tables |  |
| Suspend WP Options Autoload |  |
| Post Missed Schedule Tweaks |  |
| Misc Performance Tweaks |  |
| Misc WooCommerce Tweaks |  |
| Deactivate WooCommerce Admin |  |
| Deactivate WooCommerce Widget |  |
| Deactivate WooCommerce WP Dashboard |  |
| Remove XML-RPC / Pingbacks |  |
| Remove WP Header Junk |  |
| Remove WP Emoji |  |
| Remove WP Feed |  |
| Remove WP Embed |  |
| Remove WP Lazy Load |  |
| Remove WP Sitemap |  |
| Admin Page Cache Preloading |  |
| Admin Page Loader |  |
| Object Cache Data Stats |  |
| Garbage Collector Action Button |  |
| Docket Cache Auto Update |  |
| Check Critical Version |  |
|  |  |

## Cache Log

draft

## Cronbot

draft



