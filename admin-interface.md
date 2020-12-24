---
description: Docket Cache WP Admin Interface
---

# Admin Interface

`Updated: 07-Dec-2020 | Version: >= 20.10.10`

The Docket Cache keeps the admin interface clean, responsive and simple as possible, predefined configuration and reuses WordPress library as much as possible.

## Overview

The Overview section is a central place to view the current status of Docket Cache activities, configuration and other useful information.

| Label | Description |
| :--- | :--- |
| **Web Server** | Web Server name. |
| **PHP SAPI** | PHP version and type of Server API. |
| **Cloudflare** | Cloudflare IP and Ray ID. \(1\) |
| **Web Proxy** | Web Proxy IP other than Cloudflare. \(2\) |
| **Object Cache Stats** | Total object size in cache files. |
| **Object OPcache Stats** | Total OPcache size in memory, for objects cache files. |
| **WP OPcache Stats** | Total OPcache size in memory, for WordPress files. |
| **PHP Memory Limit** | Your Server PHP memory limit setting. |
| **WP Frontend Memory Limit** | WordPress Website memory limit. |
| **WP Backend Memory Limit** | WordPress Admin memory limit. |
| **WP Multi Site** | Status either is Multisite. \(3\) |
| **WP Multi Network** | Status either is Multi-Network. \(4\) |
| **Primary Network** | Status either is Primary Network. \(4\) |
| **Network Locking File** | Network Lock file. \(4\) |
| **Drop-in Writable** | Status either Drop-in file can be written, replace or delete. |
| **Drop-in use Wrapper** | Status either Drop-in file is wrapper file. \(5\) |
| **Drop-in Wrapper Available** | Status either Drop-in wrapper file exists. \(5\) |
| **Drop-in Wrapper File** | Drop-in wrapper file location. \(5\) |
| **Drop-in File** | Drop-in file path. |
| **Cache Writable** | Status either cache file can be written, replace or delete. |
| **Cache Files Limit** | Current total cache files and maximum files can be store on disk. |
| **Cache Disk Limit** | Current total size cache files and maximum size can be store on disk. |
| **Cache Path** | Cache directory path. |
| **Config Writable** | Status either config file can be written, replace or delete. |
| **Config Path** | Config directory path. |

{% hint style="info" %}
1. Only visible if your website running behind Cloudflare.
2. Only visible if web proxy is not Cloudflare such Sucuri and Varnish.
3. Only visible in Multisite single-network.
4. Only visible in Multisite Multi-Network setup.
5. Only visible if `DOCKET_CACHE_CONTENT_PATH` constant defined.
{% endhint %}

## Configuration

The configuration section allows you to change the Docket Cache behaviour without using constant variables. If related constant defined in the `wp-config.php` file, it will overwrite the changes in this section.

| Label | Related Constant |
| :--- | :--- |
| **Cronbot Service** | [DOCKET\_CACHE\_CRONBOT](https://docs.docketcache.com/constants#docket_cache_cronbot) |
| **Cache Log** | [DOCKET\_CACHE\_LOG](https://docs.docketcache.com/constants#docket_cache_log) |
| **Advanced Post Caching** | [DOCKET\_CACHE\_ADVCPOST](https://docs.docketcache.com/constants#docket_cache_advcpost) |
| **Object Cache Precaching** | [DOCKET\_CACHE\_PRECACHE](https://docs.docketcache.com/constants#docket_cache_precache) |
| **WordPress Translation Caching** | [DOCKET\_CACHE\_MOCACHE](https://docs.docketcache.com/constants#docket_cache_mocache) |
| **Optimize WP Query** | [DOCKET\_CACHE\_OPTWPQUERY](https://docs.docketcache.com/constants#docket_cache_optwpquery) |
| **Optimize Term Count Queries** | [DOCKET\_CACHE\_OPTERMCOUNT](https://docs.docketcache.com/constants#docket_cache_optermcount) |
| **Optimize Database Tables** | [DOCKET\_CACHE\_CRONOPTMZDB](https://docs.docketcache.com/constants#docket_cache_cronoptmzdb) |
| **Suspend WP Options Autoload** | [DOCKET\_CACHE\_WPOPTALOAD](https://docs.docketcache.com/constants#docket_cache_wpoptaload) |
| **Post Missed Schedule Tweaks** | [DOCKET\_CACHE\_POSTMISSEDSCHEDULE](https://docs.docketcache.com/constants#docket_cache_postmissedschedule) |
| **Misc Performance Tweaks** | [DOCKET\_CACHE\_MISC\_TWEAKS](https://docs.docketcache.com/constants#docket_cache_misc_tweaks) |
| **Misc WooCommerce Tweaks** | [DOCKET\_CACHE\_WOOTWEAKS](https://docs.docketcache.com/constants#docket_cache_wootweaks) |
| **Deactivate WooCommerce Admin** | [DOCKET\_CACHE\_WOOADMINOFF](https://docs.docketcache.com/constants#docket_cache_wooadminoff) |
| **Deactivate WooCommerce Widget** | [DOCKET\_CACHE\_WOOWIDGETOFF](https://docs.docketcache.com/constants#docket_cache_woowidgetoff) |
| **Deactivate WooCommerce WP Dashboard** | [DOCKET\_CACHE\_WOOWPDASHBOARDOFF](https://docs.docketcache.com/constants#docket_cache_woowpdashboardoff) |
| **Deactivate WooCommerce Cart Fragments** | [DOCKET\_CACHE\_WOOCARTFRAGSOFF](https://docs.docketcache.com/constants#docket_cache_woocartfragsoff) |
| **Remove XML-RPC / Pingbacks** | [DOCKET\_CACHE\_PINGBACK](https://docs.docketcache.com/constants#docket_cache_pingback) |
| **Remove WP Header Junk** | [DOCKET\_CACHE\_HEADERJUNK](https://docs.docketcache.com/constants#docket_cache_headerjunk) |
| **Remove WP Emoji** | [DOCKET\_CACHE\_WPEMOJI](https://docs.docketcache.com/constants#docket_cache_wpemoji) |
| **Remove WP Feed** | [DOCKET\_CACHE\_WPFEED](https://docs.docketcache.com/constants#docket_cache_wpfeed) |
| **Remove WP Embed** | [DOCKET\_CACHE\_WPEMBED](https://docs.docketcache.com/constants#docket_cache_wpembed) |
| **Remove WP Lazy Load** | [DOCKET\_CACHE\_WPLAZYLOAD](https://docs.docketcache.com/constants#docket_cache_wplazyload) |
| **Remove WP Sitemap** | [DOCKET\_CACHE\_WPSITEMAP](https://docs.docketcache.com/constants#docket_cache_wpsitemap) |
| **Remove WP Application Passwords** | [DOCKET\_CACHE\_WPAPPPASSWORD](https://docs.docketcache.com/constants#docket_cache_wpapppassword) |
| **Admin Page Cache Preloading** | [DOCKET\_CACHE\_PRELOAD](https://docs.docketcache.com/constants#docket_cache_preload) |
| **Admin Page Loader** | [DOCKET\_CACHE\_PAGELOADER](https://docs.docketcache.com/constants#docket_cache_pageloader) |
| **Object Cache Data Stats** | [DOCKET\_CACHE\_STATS](https://docs.docketcache.com/constants#docket_cache_stats) |
| **Garbage Collector Action Button** | [DOCKET\_CACHE\_GCACTION](https://docs.docketcache.com/constants#docket_cache_gcaction) |
| **Docket Cache Auto Update** | [DOCKET\_CACHE\_AUTOUPDATE](https://docs.docketcache.com/constants#docket_cache_autoupdate) |
| **Check Critical Version** | [DOCKET\_CACHE\_CHECKVERSION](https://docs.docketcache.com/constants#docket_cache_checkversion) |

## Cache Log

The cache log section allows you to view cache log for debugging and monitor cache activities. This section only visible if Cache Log option enabled at the configuration section.

| Label | Description |
| :--- | :--- |
| **Timestamp** | Timestamp format. |
| **Log All** | Enable or Disable Log All. |
| **Log File** | Log file path. |
| **Log Size** | Log file size. |
| **Flush Log** | Flush log file. |

{% hint style="info" %}
Please refer to [`DOCKET_CACHE_LOG*`](https://docs.docketcache.com/constants#docket_cache_log) related constant for details.
{% endhint %}

## Cronbot

The cronbot section allows you to connect Docket Cache with Cronbot Service. This section also provides a function to view and execute registered cron task.

| Label | Description |
| :--- | :--- |
| **Service Status** | Status either connected to Cronbot Service. |
| **Last Received Ping** | Timestamp last Cronbot Service connect to your website. |
| **Next Expecting Ping** | Timestamp next Cronbot Service expected connect to your website. |
| **Connect** | Connect to Cronbot Service. |
| **Disconnect** | Disconnect from Cronbot Service. |
| **Run Scheduled Event** | Execute scheduled cron task. |
| **Run All Now** | Execute all cron task. |

{% hint style="info" %}
Please refer to [`DOCKET_CACHE_CRONBOT`](https://docs.docketcache.com/constants#docket_cache_cronbot) constant for details.
{% endhint %}

