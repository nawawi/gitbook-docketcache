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
      <td style="text-align:left">Drop-in File</td>
      <td style="text-align:left">Drop-in file location.</td>
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
      <td style="text-align:left">Cache directory.</td>
    </tr>
    <tr>
      <td style="text-align:left">Config Writable</td>
      <td style="text-align:left">Status either config file can be written, replace or delete.</td>
    </tr>
    <tr>
      <td style="text-align:left">Config Path</td>
      <td style="text-align:left">Config location.</td>
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
| Cronbot Service | DOCKET\_CACHE\_CRONBOT |
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



