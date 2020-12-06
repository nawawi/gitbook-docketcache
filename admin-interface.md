---
description: Docket Cache WP Admin Interface
---

# Admin Interface

`Updated: 05-Dec-2020 | Version: >= 20.10.10`

The Docket Cache keeps the admin interface clean, responsive and simple as possible, predefined configuration and reuses WordPress library as much as possible.

## Overview

The Overview section is a central place to view the current status of Docket Cache activities, configuration and other useful information.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Label</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Web Server</td>
      <td style="text-align:left">Web Server name.</td>
    </tr>
    <tr>
      <td style="text-align:left">PHP SAPI</td>
      <td style="text-align:left">PHP version and type of Server API.</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloudflare</td>
      <td style="text-align:left">
        <p>Cloudflare IP and Ray ID.</p>
        <p>Only visible if your website running behind Cloudflare.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Web Proxy</td>
      <td style="text-align:left">
        <p>Web Proxy IP other than Cloudflare.</p>
        <p>Only visible if web proxy is not Cloudflare, like Sucuri.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Object Cache Stats</td>
      <td style="text-align:left">Total object size in cache files.</td>
    </tr>
    <tr>
      <td style="text-align:left">Object OPcache Stats</td>
      <td style="text-align:left">Total OPcache size in memory, for objects cache files.</td>
    </tr>
    <tr>
      <td style="text-align:left">WP OPcache Stats</td>
      <td style="text-align:left">Total OPcache size in memory, for WordPress files.</td>
    </tr>
    <tr>
      <td style="text-align:left">PHP Memory Limit</td>
      <td style="text-align:left">Your Server PHP memory limit setting.</td>
    </tr>
    <tr>
      <td style="text-align:left">WP Frontend Memory Limit</td>
      <td style="text-align:left">WordPress Website memory limit.</td>
    </tr>
    <tr>
      <td style="text-align:left">WP Backend Memory Limit</td>
      <td style="text-align:left">WordPress Admin memory limit.</td>
    </tr>
    <tr>
      <td style="text-align:left">WP Multi Site</td>
      <td style="text-align:left">Status either is Multisite. Only visible in Multisite single-network.</td>
    </tr>
    <tr>
      <td style="text-align:left">WP Multi Network</td>
      <td style="text-align:left">
        <p>Status either is Multi-Network.</p>
        <p>Only visible in Multisite Multi-Network setup.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Primary Network</td>
      <td style="text-align:left">
        <p>Status either is Primary Network.</p>
        <p>Only visible in Multisite Multi-Network setup.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Network Locking File</td>
      <td style="text-align:left">
        <p>Network Lock file.</p>
        <p>Only visible in Multisite Multi-Network setup.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Drop-in Writable</td>
      <td style="text-align:left">Status either Drop-in file can be written, replace or delete.</td>
    </tr>
    <tr>
      <td style="text-align:left">Drop-in use Wrapper</td>
      <td style="text-align:left">
        <p>Status either Drop-in file is wrapper file.</p>
        <p>Only visible if <code>DOCKET_CACHE_CONTENT_PATH</code> constant was defined.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Drop-in Wrapper Available</td>
      <td style="text-align:left">
        <p>Status either Drop-in wrapper file exists.</p>
        <p>Only visible if <code>DOCKET_CACHE_CONTENT_PATH</code> constant was defined.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Drop-in Wrapper File</td>
      <td style="text-align:left">
        <p>Drop-in wrapper file location.</p>
        <p>Only visible if <code>DOCKET_CACHE_CONTENT_PATH</code> constant was defined.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Drop-in File</td>
      <td style="text-align:left">Drop-in file path.</td>
    </tr>
    <tr>
      <td style="text-align:left">Cache Writable</td>
      <td style="text-align:left">Status either cache file can be written, replace or delete.</td>
    </tr>
    <tr>
      <td style="text-align:left">Cache Files Limit</td>
      <td style="text-align:left">Current total cache files and maximum files can be store on disk.</td>
    </tr>
    <tr>
      <td style="text-align:left">Cache Disk Limit</td>
      <td style="text-align:left">Current total size cache files and maximum size can be store on disk.</td>
    </tr>
    <tr>
      <td style="text-align:left">Cache Path</td>
      <td style="text-align:left">Cache directory path.</td>
    </tr>
    <tr>
      <td style="text-align:left">Config Writable</td>
      <td style="text-align:left">Status either config file can be written, replace or delete.</td>
    </tr>
    <tr>
      <td style="text-align:left">Config Path</td>
      <td style="text-align:left">Config directory path.</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

## Configuration

The configuration section allows you to change the Docket Cache behaviour without using constant variables. If related constant defined in the `wp-config.php` file, it will overwrite the changes in this section.

| Label | Related Constant |
| :--- | :--- |
| Cronbot Service | \[[`DOCKET_CACHE_CRONBOT`](https://docs.docketcache.com/constants#docket_cache_cronbot)\] |
| Cache Log | `DOCKET_CACHE_LOG` |
| Advanced Post Caching | `DOCKET_CACHE_ADVCPOST` |
| Object Cache Precaching | `DOCKET_CACHE_PRECACHE` |
| WordPress Translation Caching | `DOCKET_CACHE_MOCACHE` |
| Optimize WP Query | `DOCKET_CACHE_OPTWPQUERY` |
| Optimize Term Count Queries | `DOCKET_CACHE_OPTERMCOUNT` |
| Optimize Database Tables | `DOCKET_CACHE_CRONOPTMZDB` |
| Suspend WP Options Autoload | `DOCKET_CACHE_WPOPTALOAD` |
| Post Missed Schedule Tweaks | `DOCKET_CACHE_POSTMISSEDSCHEDULE` |
| Misc Performance Tweaks | `DOCKET_CACHE_MISC_TWEAKS` |
| Misc WooCommerce Tweaks | `DOCKET_CACHE_WOOTWEAKS` |
| Deactivate WooCommerce Admin | `DOCKET_CACHE_WOOADMINOFF` |
| Deactivate WooCommerce Widget | `DOCKET_CACHE_WOOWIDGETOFF` |
| Deactivate WooCommerce WP Dashboard | `DOCKET_CACHE_WOOWPDASHBOARDOFF` |
| Remove XML-RPC / Pingbacks | `DOCKET_CACHE_PINGBACK` |
| Remove WP Header Junk | `DOCKET_CACHE_HEADERJUNK` |
| Remove WP Emoji | `DOCKET_CACHE_WPEMOJI` |
| Remove WP Feed | `DOCKET_CACHE_WPFEED` |
| Remove WP Embed | `DOCKET_CACHE_WPEMBED` |
| Remove WP Lazy Load | `DOCKET_CACHE_WPLAZYLOAD` |
| Remove WP Sitemap | `DOCKET_CACHE_WPSITEMAP` |
| Admin Page Cache Preloading | `DOCKET_CACHE_PRELOAD` |
| Admin Page Loader | `DOCKET_CACHE_PAGELOADER` |
| Object Cache Data Stats | `DOCKET_CACHE_STATS` |
| Garbage Collector Action Button | `DOCKET_CACHE_GCACTION` |
| Docket Cache Auto Update | `DOCKET_CACHE_AUTOUPDATE` |
| Check Critical Version | `DOCKET_CACHE_CHECKVERSION` |

## Cache Log

draft

## Cronbot

draft



