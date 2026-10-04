---
description: Docket Cache WP Admin Interface
---

# Admin Interface

`Updated: 04-Oct-2026 | v26.04.07`

The Docket Cache keeps the admin interface clean, responsive and as simple as possible, with predefined configurations and reusing WordPress libraries as much as possible.

## Overview

The Overview screen is the primary place to view the current status of Docket Cache activity, configuration, and other useful information.

| Label                         | Description                                                           |
| ----------------------------- | --------------------------------------------------------------------- |
| **Web Server**                | Web Server name.                                                      |
| **PHP SAPI**                  | PHP version and type of Server API.                                   |
| **Cloudflare**                | Cloudflare IP and Ray ID. (1)                                         |
| **Web Proxy**                 | Web Proxy IP other than Cloudflare. (2)                               |
| **Object Cache Stats**        | Total object size in cache files.                                     |
| **Object OPcache Stats**      | Total OPcache size in memory, for objects cache files.                |
| **WP OPcache Stats**          | Total OPcache size in memory, for WordPress files.                    |
| **Object Cache**              | Object Cache status. (6)                                              |
| **Zend OPcache**              | Zend OPcache status. (6)                                              |
| **PHP Memory Limit**          | Your Server PHP memory limit setting.                                 |
| **WP Frontend Memory Limit**  | WordPress Website memory limit.                                       |
| **WP Backend Memory Limit**   | WordPress Admin memory limit.                                         |
| **WP Multi Site**             | Status either is Multisite. (3)                                       |
| **WP Multi Network**          | Status either is Multi-Network. (4)                                   |
| **Primary Network**           | Status either is Primary Network. (4)                                 |
| **Network Locking File**      | Network Lock file. (4)                                                |
| **Drop-in Writable**          | Status either Drop-in file can be written, replace or delete.         |
| **Drop-in File**              | Drop-in file path.                                                    |
| **Drop-in use Wrapper**       | Status either Drop-in file is wrapper file. (5)                       |
| **Drop-in Wrapper Available** | Status either Drop-in wrapper file exists. (5)                        |
| **Drop-in Wrapper File**      | Drop-in wrapper file location. (5)                                    |
| **Cache Writable**            | Status either cache file can be written, replace or delete.           |
| **Cache Files Limit**         | Current total cache files and maximum files can be store on disk.     |
| **Cache Disk Limit**          | Current total size cache files and maximum size can be store on disk. |
| **Cache Path**                | Cache directory path.                                                 |
| **Chunk Cache Directory**     | Status either the Chunk Cache Directory option is enabled.            |
| **Config Writable**           | Status either config file can be written, replace or delete.          |
| **Config Path**               | Config directory path.                                                |

{% hint style="info" %}
1. Only visible if your website running behind Cloudflare.
2. Only visible if web proxy is not Cloudflare such Sucuri and Varnish.
3. Only visible in Multisite single-network.
4. Only visible in Multisite Multi-Network setup.
5. Only visible if `DOCKET_CACHE_CONTENT_PATH` constant defined.
6. Shown in place of Object Cache Stats, Object OPcache Stats and WP OPcache Stats when those stats are not available.
{% endhint %}

#### ACTIONS

| Label                    | Description                                                                |
| ------------------------ | -------------------------------------------------------------------------- |
| **Flush Object Cache**   | Remove all cache files.                                                    |
| **Flush OPcache**        | Reset OPcache usage.                                                       |
| **Disable Object Cache** | Disable Drop-In usage. Shown when the Drop-In is in use.                   |
| **Enable Object Cache**  | Enable Drop-In usage. Shown when the Drop-In is not in use.                |
| **Run Garbage Collector**| Execute the Garbage Collector Task. (1)                                    |

{% hint style="info" %}
1. Only visible if the Garbage Collector Action Button option is enabled on the configuration screen.
{% endhint %}

## Cronbot

The cronbot screen allows you to connect Docket Cache with Cronbot Service. This screen also provides a function to view and execute registered cron tasks.

| Label                   | Description                                                      |
| ----------------------- | ---------------------------------------------------------------- |
| **Service Status**      | Status either connected to Cronbot Service.                      |
| **Last Received Ping**  | Timestamp last Cronbot Service connect to your website.          |
| **Next Expecting Ping** | Timestamp next Cronbot Service expected connect to your website. Only visible when connected. |
| **Connect**             | Connect to Cronbot Service.                                      |
| **Disconnect**          | Disconnect from Cronbot Service.                                 |
| **Test Ping**           | Send a test ping to Cronbot Service. Only visible when connected. |
| **Cron Events For Site** | The site whose cron events are listed. In Multisite with more than one site, a list to select the site. |
| **Run Scheduled Event** | Execute scheduled cron task.                                     |
| **Run All Now**         | Execute all cron task.                                           |
| **Filter Hook Names**   | Search the cron events by hook name. Only visible when the list has more than one page. |

The Cron Events list has the following columns:

| Label             | Description                                        |
| ----------------- | -------------------------------------------------- |
| **Hook**          | Hook name of the cron event.                       |
| **Arguments**     | Arguments of the cron event.                       |
| **Next Schedule** | Time of the next run, with the UTC offset.         |
| **Action**        | Run Now, to execute this cron event.               |
| **Recurrence**    | Recurrence of the cron event, or Non-repeating.    |

{% hint style="info" %}
Please refer to [`DOCKET_CACHE_CRONBOT`](https://docs.docketcache.com/constants#docket\_cache\_cronbot) constant for details.
{% endhint %}

## OPcache

The OPcache screen allows you to view OPcache status and usage. This screen is only visible if the OPcache Viewer option is enabled on the configuration screen.

#### OPCACHE USAGE

| Label                    | Description                                                    |
| ------------------------ | -------------------------------------------------------------- |
| **Cache Hits**           | Total OPcache hits.                                            |
| **Cache Misses**         | Total OPcache misses.                                          |
| **Cached Files**         | Total cached files.                                            |
| **Cached Keys**          | Total cached keys.                                             |
| **Max Cached Keys**      | Maximum cached keys.                                           |
| **Hit Rate**             | OPcache hit rate in percent.                                   |
| **Blacklist Misses**     | Total blacklist misses. (1)                                    |
| **Blacklist Miss Ratio** | Blacklist miss ratio in percent. (1)                           |
| **Used Memory**          | OPcache used memory.                                           |
| **Free Memory**          | OPcache free memory.                                           |
| **Wasted Memory**        | OPcache wasted memory.                                         |
| **Current Wasted**       | Current wasted memory in percent.                              |
| **Status**               | File Cache Only. (2)                                           |
| **File Cache Path**      | OPcache file cache path. (2)                                   |
| **Stats**                | Total size and number of the file cache files. (2)             |
| **Flush OPcache**        | Reset OPcache usage.                                           |
| **Display Config**       | Display the OPcache Config screen.                             |

{% hint style="info" %}
1. Only visible if Blacklist Misses is not zero.
2. Only visible if OPcache runs in File Cache Only mode, in place of the Statistics and Memory Usage labels.
{% endhint %}

#### OPCACHE CONFIG

| Label          | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| **Version**    | OPcache version information.                                             |
| **Directives** | OPcache configuration directives, each linked to its documentation.      |
| **Dismiss**    | Return to the OPcache Usage screen.                                      |

#### OPCACHE FILES

| Label                   | Description                                                                                         |
| ----------------------- | --------------------------------------------------------------------------------------------------- |
| **Object Cache Files**  | List the Object Cache files. This is the default.                                                   |
| **Other Files**         | List the other files.                                                                               |
| **Stale Files**         | List the stale files. (1)                                                                           |
| **All**                 | List all files.                                                                                     |
| **< 1000 Items**        | Limit of the items to list. The choices are 1000, 5000, 10000 and 50000 items.                      |
| **Filter Cached Files** | Search the cached files by path. Only visible when the list has more than one page.                 |
| **Cached Files**        | Path of the cached file.                                                                            |
| **Cache Hits**          | Total hits of the cached file. (1)                                                                  |
| **Memory Usage**        | Memory usage of the cached file. In File Cache Only mode, this column is File Size.                 |
| **Last Used**           | Timestamp the cached file was last used, with the UTC offset.                                       |

{% hint style="info" %}
1. Not visible if OPcache runs in File Cache Only mode.

By default, only files within the WordPress installation path are listed. Please refer to [DOCKET\_CACHE\_OPCVIEWER\_SHOWALL](constants.md#docket\_cache\_opcviewer\_showall) constant for details.
{% endhint %}

## Cache Log

The cache log screen allows you to view the cache log for debugging and monitor cache activities. This screen is only visible if the Cache Log option is enabled on the configuration screen.

| Label         | Description                |
| ------------- | -------------------------- |
| **Timestamp** | Timestamp format.          |
| **Log All**   | Enable or Disable Log All. |
| **Log File**  | Log file path.             |
| **Log Size**  | Log file size.             |
| **Flush Log** | Flush log file.            |
| **Refresh**   | Reload the cache log.      |
| **View**      | Open the Cache View for the selected log row. |

The lists above the Flush Log button select the FIRST or LAST 10, 50, 100, 300 or 500 lines of the log file, shown in ASC or DESC order.

Select a row in the log to view the cache content. The Cache View shows the following:

| Label           | Description                              |
| --------------- | ---------------------------------------- |
| **Cache Index** | Index of the cache file being viewed.    |
| **Cache Size**  | Cache data size and the maximum size.    |
| **Flush**       | Flush the cache file being viewed.       |
| **Close**       | Return to the cache log.                 |

{% hint style="info" %}
Please refer to [`DOCKET_CACHE_LOG*`](https://docs.docketcache.com/constants#docket\_cache\_log) related constant for details.
{% endhint %}

## Configuration

The configuration screen allows you to change the Docket Cache behaviour without using constant variables. If related constants are defined in the `wp-config.php` file, it will overwrite the changes on this screen.

#### FEATURE OPTIONS

| Label               | Related Constant                                                                        |
| ------------------- | --------------------------------------------------------------------------------------- |
| **Cronbot Service** | [DOCKET\_CACHE\_CRONBOT](https://docs.docketcache.com/constants#docket\_cache\_cronbot) |
| **OPcache Viewer**  | [DOCKET\_CACHE\_OPCVIEWER](constants.md#docket\_cache\_opcviewer) |
| **Cache Log**       | [DOCKET\_CACHE\_LOG](https://docs.docketcache.com/constants#docket\_cache\_log)         |

#### CACHE OPTIONS

| Label                             | Related Constant                                                                                |
| --------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Advanced Post Caching** (1)     | [DOCKET\_CACHE\_ADVCPOST](https://docs.docketcache.com/constants#docket\_cache\_advcpost)       |
| **Post Caching Any Post Type** (2) | [DOCKET\_CACHE\_ADVCPOST\_POSTTYPE\_ALL](constants.md#docket\_cache\_advcpost\_posttype\_all) |
| **Object Cache Precaching**       | [DOCKET\_CACHE\_PRECACHE](https://docs.docketcache.com/constants#docket\_cache\_precache)       |
| **WordPress Menu Caching**        | [DOCKET\_CACHE\_MENUCACHE](constants.md#docket\_cache\_menucache) |
| **WordPress Translation Caching** | [DOCKET\_CACHE\_MOCACHE](https://docs.docketcache.com/constants#docket\_cache\_mocache)         |
| **Admin Page Cache Preloading**   | [DOCKET\_CACHE\_PRELOAD](https://docs.docketcache.com/constants#docket\_cache\_preload)         |
| **Retain Transients in Db**       | [DOCKET\_CACHE\_TRANSIENTDB](https://docs.docketcache.com/constants#docket\_cache\_transientdb) |

{% hint style="info" %}
1. Only visible if the WordPress version is lower than 6.1.
2. Only visible if the WordPress version is lower than 6.1 and Advanced Post Caching is enabled.
{% endhint %}

#### OPTIMISATIONS

| Label                           | Related Constant                                                                                              |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Optimize WP Query**           | [DOCKET\_CACHE\_OPTWPQUERY](https://docs.docketcache.com/constants#docket\_cache\_optwpquery)                 |
| **Optimize Term Count Queries** | [DOCKET\_CACHE\_OPTERMCOUNT](https://docs.docketcache.com/constants#docket\_cache\_optermcount)               |
| **Optimize Database Tables**    | [DOCKET\_CACHE\_CRONOPTMZDB](https://docs.docketcache.com/constants#docket\_cache\_cronoptmzdb)               |
| **Suspend WP Options Autoload** | [DOCKET\_CACHE\_WPOPTALOAD](https://docs.docketcache.com/constants#docket\_cache\_wpoptaload)                 |
| **Post Missed Schedule Tweaks** | [DOCKET\_CACHE\_POSTMISSEDSCHEDULE](https://docs.docketcache.com/constants#docket\_cache\_postmissedschedule) |
| **Limit Bulk Edit Actions**     | [DOCKET\_CACHE\_LIMITBULKEDIT](https://docs.docketcache.com/constants#docket\_cache\_limitbulkedit)           |
| **Misc Performance Tweaks**     | [DOCKET\_CACHE\_MISC\_TWEAKS](https://docs.docketcache.com/constants#docket\_cache\_misc\_tweaks)             |

#### WOO TWEAKS

| Label                                         | Related Constant                                                                                                    |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Misc WooCommerce Tweaks**                   | [DOCKET\_CACHE\_WOOTWEAKS](https://docs.docketcache.com/constants#docket\_cache\_wootweaks)                         |
| **Deactivate WooCommerce Admin**              | [DOCKET\_CACHE\_WOOADMINOFF](https://docs.docketcache.com/constants#docket\_cache\_wooadminoff)                     |
| **Deactivate WooCommerce Classic Widget**     | [DOCKET\_CACHE\_WOOWIDGETOFF](https://docs.docketcache.com/constants#docket\_cache\_woowidgetoff)                   |
| **Deactivate WooCommerce WP Dashboard**       | [DOCKET\_CACHE\_WOOWPDASHBOARDOFF](https://docs.docketcache.com/constants#docket\_cache\_woowpdashboardoff)         |
| **Deactivate WooCommerce Extensions Page**    | [DOCKET\_CACHE\_WOOEXTENSIONPAGEOFF](https://docs.docketcache.com/constants#docket\_cache\_wooextensionpageoff)     |
| **Deactivate WooCommerce Cart Fragments**     | [DOCKET\_CACHE\_WOOCARTFRAGSOFF](https://docs.docketcache.com/constants#docket\_cache\_woocartfragsoff)             |
| **Prevent robots crawling add-to-cart links** | [DOCKET\_CACHE\_WOOADDTOCHARTCRAWLING](https://docs.docketcache.com/constants#docket\_cache\_wooaddtochartcrawling) |

#### WP TWEAKS

| Label                                          | Related Constant                                                                                          |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Deactivate WP Header Junk**                  | [DOCKET\_CACHE\_HEADERJUNK](constants.md#docket\_cache\_headerjunk) |
| **Deactivate XML-RPC / Pingbacks**             | [DOCKET\_CACHE\_PINGBACK](constants.md#docket\_cache\_pingback) |
| **Deactivate WP Emoji**                        | [DOCKET\_CACHE\_WPEMOJI](https://docs.docketcache.com/constants#docket\_cache\_wpemoji)                   |
| **Deactivate WP Feed**                         | [DOCKET\_CACHE\_WPFEED](https://docs.docketcache.com/constants#docket\_cache\_wpfeed)                     |
| **Deactivate WP Embed**                        | [DOCKET\_CACHE\_WPEMBED](https://docs.docketcache.com/constants#docket\_cache\_wpembed)                   |
| **Deactivate WP Lazy Load**                    | [DOCKET\_CACHE\_WPLAZYLOAD](https://docs.docketcache.com/constants#docket\_cache\_wplazyload)             |
| **Deactivate WP Sitemap**                      | [DOCKET\_CACHE\_WPSITEMAP](https://docs.docketcache.com/constants#docket\_cache\_wpsitemap)               |
| **Deactivate WP Application Passwords**        | [DOCKET\_CACHE\_WPAPPPASSWORD](https://docs.docketcache.com/constants#docket\_cache\_wpapppassword)       |
| **Deactivate WP Events & News Feed Dashboard** | [DOCKET\_CACHE\_WPDASHBOARDNEWS](https://docs.docketcache.com/constants#docket\_cache\_wpdashboardnews)   |
| **Deactivate Post Via Email**                  | [DOCKET\_CACHE\_POSTVIAEMAIL](https://docs.docketcache.com/constants#docket\_cache\_postviaemail)         |
| **Deactivate Browse Happy Checking**           | [DOCKET\_CACHE\_WPBROWSEHAPPY](https://docs.docketcache.com/constants#docket\_cache\_wpbrowsehappy)       |
| **Deactivate Serve Happy Checking**            | [DOCKET\_CACHE\_WPSERVEHAPPY](https://docs.docketcache.com/constants#docket\_cache\_wpservehappy)         |
| **Limit WP-Admin HTTP requests**               | [DOCKET\_CACHE\_LIMITHTTPREQUEST](constants.md#docket\_cache\_limithttprequest) |
| **HTTP Request Expect header tweaks** (1)      | [DOCKET\_CACHE\_HTTPHEADERSEXPECT](constants.md#docket\_cache\_httpheadersexpect) |

{% hint style="info" %}
1. Only visible if the WordPress version is lower than 5.8.
{% endhint %}

#### RUNTIME OPTIONS

| Label                                                | Related Constant |
| ---------------------------------------------------- | ---------------- |
| **Auto Save Interval**                               | [DOCKET\_CACHE\_RTPOSTAUTOSAVE](constants.md#docket\_cache\_rtpostautosave) |
| **Post Revisions**                                   | [DOCKET\_CACHE\_RTPOSTREVISION](constants.md#docket\_cache\_rtpostrevision) |
| **Trash Bin**                                        | [DOCKET\_CACHE\_RTPOSTEMPTYTRASH](constants.md#docket\_cache\_rtpostemptytrash) |
| **Cleanup Image Edits**                              | [DOCKET\_CACHE\_RTIMAGEOVERWRITE](constants.md#docket\_cache\_rtimageoverwrite) |
| **Disallows WP Auto Update Core**                    | [DOCKET\_CACHE\_RTWPCOREUPDATE](constants.md#docket\_cache\_rtwpcoreupdate) |
| **Disallows Plugin / Theme Editor**                  | [DOCKET\_CACHE\_RTPLUGINTHEMEEDITOR](constants.md#docket\_cache\_rtpluginthemeeditor) |
| **Disallows Plugin / Theme Update and Installation** | [DOCKET\_CACHE\_RTPLUGINTHEMEINSTALL](constants.md#docket\_cache\_rtpluginthemeinstall) |
| **Deactivate Concatenate WP-Admin Scripts**          | [DOCKET\_CACHE\_RTCONCATENATESCRIPTS](constants.md#docket\_cache\_rtconcatenatescripts) |
| **Deactivate WP Cron**                               | [DOCKET\_CACHE\_RTDISABLEWPCRON](constants.md#docket\_cache\_rtdisablewpcron) |
| **WP Debug**                                         | [DOCKET\_CACHE\_RTWPDEBUG](constants.md#docket\_cache\_rtwpdebug) |
| **WP Debug Display** (1)                             | [DOCKET\_CACHE\_RTWPDEBUGDISPLAY](constants.md#docket\_cache\_rtwpdebugdisplay) |
| **WP Debug Log** (1)                                 | [DOCKET\_CACHE\_RTWPDEBUGLOG](constants.md#docket\_cache\_rtwpdebuglog) |

{% hint style="info" %}
The Runtime Options are only visible on the main network and require the runtime code to be installed in the `wp-config.php` file, see Install Runtime Code in the Actions below.

1. Only visible if WP Debug is enabled.
{% endhint %}

#### STORAGE OPTIONS

| Label                             | Related Constant                                                                                              |
| ----------------------------------| ------------------------------------------------------------------------------------------------------------- |
| **Cache Files Limit**             | [DOCKET\_CACHE\_MAXFILE](https://docs.docketcache.com/constants#docket\_cache\_maxfile)                       |
| **Cache Disk Limit**              | [DOCKET\_CACHE\_MAXSIZE\_DISK](https://docs.docketcache.com/constants#docket\_cache\_maxsize\_disk)           |
| **Chunk Cache Directory**         | [DOCKET\_CACHE\_CHUNKCACHEDIR](https://docs.docketcache.com/constants#docket\_cache\_chunkcachedir)           |
| **Real-time File Limit Checking** | [DOCKET\_CACHE\_MAXFILE\_LIVECHECK](https://docs.docketcache.com/constants#docket\_cache\_maxfile\_livecheck) |
| **Auto Remove Stale Cache**       | [DOCKET\_CACHE\_FLUSH\_STALECACHE](https://docs.docketcache.com/constants#docket\_cache\_flush\_stalecache)   |
| **Exclude Empty Object Data**     | [DOCKET\_CACHE\_EMPTYCACHE\_IGNORE](https://docs.docketcache.com/constants#docket\_cache\_emptycache\_ignore) |

#### ADMIN INTERFACE

| Label                                    | Related Constant                                                                                 |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Admin Page Loader**                    | [DOCKET\_CACHE\_PAGELOADER](https://docs.docketcache.com/constants#docket\_cache\_pageloader)    |
| **Object Cache Data Stats**              | [DOCKET\_CACHE\_STATS](https://docs.docketcache.com/constants#docket\_cache\_stats)              |
| **Garbage Collector Action Button**      | [DOCKET\_CACHE\_GCACTION](https://docs.docketcache.com/constants#docket\_cache\_gcaction)        |
| **Additional Flush Cache Action Button** | [DOCKET\_CACHE\_FLUSHACTION](https://docs.docketcache.com/constants#docket\_cache\_flushaction)  |
| **Export/Import Settings Action Button** | [DOCKET\_CACHE\_CONFIGACTION](constants.md#docket\_cache\_configaction) |

#### PLUGIN OPTIONS

| Label                                      | Related Constant                                                                                        |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| **Docket Cache Auto Update**               | [DOCKET\_CACHE\_AUTOUPDATE\_TOGGLE](constants.md#docket\_cache\_autoupdate\_toggle) |
| **Critical Version Checking**              | [DOCKET\_CACHE\_CHECKVERSION](constants.md#docket\_cache\_checkversion) |
| **Flush Object Cache During Deactivation** | [DOCKET\_CACHE\_FLUSH\_SHUTDOWN](https://docs.docketcache.com/constants#docket\_cache\_flush\_shutdown) |
| **Flush OPcache During Deactivation**      | [DOCKET\_CACHE\_OPCSHUTDOWN](https://docs.docketcache.com/constants#docket\_cache\_opcshutdown)         |
| **WP-CLI OPcache Invalidation**            | [DOCKET\_CACHE\_WPCLI\_OPCACHE](constants.md#docket\_cache\_wpcli\_opcache) |

#### ACTIONS

| Label                         | Description                                                                                         |
| ----------------------------- | --------------------------------------------------------------------------------------------------- |
| **Cleanup Post**              | Cleanup Revisions, Auto Draft, Trash Bin. (1)                                                       |
| **Flush Advanced Post Cache** | Remove Advanced Post Cache files. (2)                                                               |
| **Flush Object Precache**     | Remove Object Cache Precaching files. (2)                                                           |
| **Flush Transient Cache**     | Remove transient cache files. (3)                                                                   |
| **Flush Menu Cache**          | Remove menu cache files. (2)                                                                        |
| **Flush Translation Cache**   | Remove translation cache files. (2)                                                                 |
| **Export Settings**           | Download current configuration as a JSON file. (4)                                                  |
| **Import Settings**           | Upload a previously exported JSON configuration file. (4)                                           |
| **Reset to default**          | Reset all configuration to default.                                                                 |
| **Install Runtime Code**      | Display the code that handles WordPress constants for the Runtime Options, to install it. (5)       |
| **Update Runtime Code**       | Shown in place of Install Runtime Code when the runtime code is already installed. (5)              |

{% hint style="info" %}
1. In Multisite with more than one site, the For Site list selects all sites or a single site.
2. Only visible if the Additional Flush Cache Action Button option and the related cache option are enabled.
3. Only visible if the Additional Flush Cache Action Button option is enabled.
4. Only visible if the Export/Import Settings Action Button option is enabled.
5. Only visible on the main network.
{% endhint %}
