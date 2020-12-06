---
description: Docket Cache WP Admin Interface
---

# Admin Interface

`Updated: 05-Dec-2020 | Version: >= 20.10.10`

The Docket Cache keeps the admin interface clean, responsive and simple as possible, predefined configuration and reuses WordPress library as much as possible.

## Overview

The Overview section is a central place to view the current status of Docket Cache activities, configuration and other useful information.

| Label | Description |
| :--- | :--- |
| Web Server | Web Server name. |
| PHP SAPI | PHP version and type of Server API. |
| Cloudflare | Cloudflare IP and Ray ID. \(1\) |
| Web Proxy | Web Proxy IP other than Cloudflare. \(2\) |
| Object Cache Stats | Total object size in cache files. |
| Object OPcache Stats | Total OPcache size in memory, for objects cache files. |
| WP OPcache Stats | Total OPcache size in memory, for WordPress files. |
| PHP Memory Limit | Your Server PHP memory limit setting. |
| WP Frontend Memory Limit | WordPress Website memory limit. |
| WP Backend Memory Limit | WordPress Admin memory limit. |
| WP Multi Site | Status either is Multisite. \(3\) |
| WP Multi Network | Status either is Multi-Network. \(4\) |
| Primary Network | Status either is Primary Network. \(4\) |
| Network Locking File | Network Lock file. \(4\) |
| Drop-in Writable | Status either Drop-in file can be written, replace or delete. |
| Drop-in use Wrapper | Status either Drop-in file is wrapper file. \(5\) |
| Drop-in Wrapper Available | Status either Drop-in wrapper file exists. \(5\) |
| Drop-in Wrapper File | Drop-in wrapper file location. \(5\) |
| Drop-in File | Drop-in file path. |
| Cache Writable | Status either cache file can be written, replace or delete. |
| Cache Files Limit | Current total cache files and maximum files can be store on disk. |
| Cache Disk Limit | Current total size cache files and maximum size can be store on disk. |
| Cache Path | Cache directory path. |
| Config Writable | Status either config file can be written, replace or delete. |
| Config Path | Config directory path. |

{% hint style="info" %}
1. Only visible if your website running behind Cloudflare.
2. Only visible if web proxy is not Cloudflare, like Sucuri.
3. Only visible in Multisite single-network.
4. Only visible in Multisite Multi-Network setup.
5. Only visible if DOCKET\_CACHE\_CONTENT\_PATH constant defined.
{% endhint %}

## Configuration

The configuration section allows you to change the Docket Cache behaviour without using constant variables. If related constant defined in the `wp-config.php` file, it will overwrite the changes in this section.

| Label | Related Constant |
| :--- | :--- |
| Cronbot Service | \`\`[`DOCKET_CACHE_CRONBOT`](https://docs.docketcache.com/constants#docket_cache_cronbot)\`\` |
| Cache Log | DOCKET\_CACHE\_LOG |
| Advanced Post Caching | DOCKET\_CACHE\_ADVCPOST |
| Object Cache Precaching | DOCKET\_CACHE\_PRECACHE |
| WordPress Translation Caching | DOCKET\_CACHE\_MOCACHE |
| Optimize WP Query | DOCKET\_CACHE\_OPTWPQUERY |
| Optimize Term Count Queries | DOCKET\_CACHE\_OPTERMCOUNT |
| Optimize Database Tables | DOCKET\_CACHE\_CRONOPTMZDB |
| Suspend WP Options Autoload | DOCKET\_CACHE\_WPOPTALOAD |
| Post Missed Schedule Tweaks | DOCKET\_CACHE\_POSTMISSEDSCHEDULE |
| Misc Performance Tweaks | DOCKET\_CACHE\_MISC\_TWEAKS |
| Misc WooCommerce Tweaks | DOCKET\_CACHE\_WOOTWEAKS |
| Deactivate WooCommerce Admin | DOCKET\_CACHE\_WOOADMINOFF |
| Deactivate WooCommerce Widget | DOCKET\_CACHE\_WOOWIDGETOFF |
| Deactivate WooCommerce WP Dashboard | DOCKET\_CACHE\_WOOWPDASHBOARDOFF |
| Remove XML-RPC / Pingbacks | DOCKET\_CACHE\_PINGBACK |
| Remove WP Header Junk | DOCKET\_CACHE\_HEADERJUNK |
| Remove WP Emoji | DOCKET\_CACHE\_WPEMOJI |
| Remove WP Feed | DOCKET\_CACHE\_WPFEED |
| Remove WP Embed | DOCKET\_CACHE\_WPEMBED |
| Remove WP Lazy Load | DOCKET\_CACHE\_WPLAZYLOAD |
| Remove WP Sitemap | DOCKET\_CACHE\_WPSITEMAP |
| Admin Page Cache Preloading | DOCKET\_CACHE\_PRELOAD |
| Admin Page Loader | DOCKET\_CACHE\_PAGELOADER |
| Object Cache Data Stats | DOCKET\_CACHE\_STATS |
| Garbage Collector Action Button | DOCKET\_CACHE\_GCACTION |
| Docket Cache Auto Update | DOCKET\_CACHE\_AUTOUPDATE |
| Check Critical Version | DOCKET\_CACHE\_CHECKVERSION |

## Cache Log

draft

## Cronbot

draft



