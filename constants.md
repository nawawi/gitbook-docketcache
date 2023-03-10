---
description: Docket Cache uses constants variable as main configuration methods.
---

# Constants

`Updated: 08-Mar-2023 | v22.07.04`

Constants are like variables except that once they are defined they cannot be changed or undefined. To change the behaviour of Docket Cache, the following PHP constants can be defined in your `wp-config.php` file.

Docket Cache load the configuration by calling [`Constan::register_default()`](https://github.com/nawawi/docket-cache/blob/master/includes/src/Constans.php#L155) method that can be found in file [includes/src/Constans.php](https://github.com/nawawi/docket-cache/blob/master/includes/src/Constans.php). Some constant marks as @private and for internal use, changing it may result in unpredictable behaviour.

## DOCKET\_CACHE\_MAXTTL

Default object lifespan in seconds.

Only numbers between 86400 and 2419200 are allowed.\
Default: 345600 (4 days)

```php
define('DOCKET_CACHE_MAXTTL', 345600);
```

If there is no expire time was set to object or set to 0, Docket Cache will use this setting as an expiration time.

This setting does not apply to cache groups below if the value of seconds is lower than the predefined seconds.

| **Group**      | **Key**                                       | **Seconds**       |
| -------------- | --------------------------------------------- | ----------------- |
| site-transient | update\_plugins, update\_themes, update\_core | 2419200 (28 days) |
| site-transient | any                                           | 604800 (7 days)   |
| transient      | any                                           | 604800 (7 days)   |
| terms          | any                                           | 1209600 (14 days) |
| posts          | any                                           | 1209600 (14 days) |
| post\_meta     | any                                           | 1209600 (14 days) |
| comments       | any                                           | 1209600 (14 days) |
| options        | any                                           | 1209600 (14 days) |
| site-options   | any                                           | 1209600 (14 days) |

## DOCKET\_CACHE\_MAXSIZE

Set the maximum size of the object data in bytes, which can be store in a cache file.

Only size between 1048576 (1MB) and 10485760 (10MB) are allowed.\
Default: 3145728 (3MB)

```php
define('DOCKET_CACHE_MAXSIZE', 3145728);
```

#### Example of object data:

```php
[
    1606534363 => [
        'wp_version_check' => [
            '40cd750bba9870f18aada2478b24840a' => [
                'schedule' => 'twicedaily',
                'args' => [],
                'interval' => 43200,
            ],
        ],
    ],
    'version' => 2,
]
```

{% hint style="info" %}
The size of the cache file is slightly bigger than the object since it contains Docket Cache metadata and exported as plain PHP code.
{% endhint %}

#### Example of the cache file content:

```php
[
    'timestamp' => 1606492052,
    'site_id' => 1,
    'group' => 'options',
    'key' => 'cron',
    'type' => 'string',
    'timeout' => 1607701652,
    'data' => [
        1606534363 => [
            'wp_version_check' => [
                '40cd750bba9870f18aada2478b24840a' => [
                    'schedule' => 'twicedaily',
                    'args' => [],
                    'interval' => 43200,
                ],
            ],
        ],
        'version' => 2,
    ],
]
```

#### Docket Cache Metadata:

| Name        | Description               |
| ----------- | ------------------------- |
| timestamp   | Data creation time        |
| site\_id    | Site Id                   |
| network\_id | Network Id (on multisite) |
| group       | Object Cache group        |
| key         | Object Cache key          |
| type        | Object Cache Data type    |
| timeout     | Expiration time           |
| data        | Object Cache data         |

## DOCKET\_CACHE\_MAXSIZE\_DISK

Set the maximum size of the cache storage on disk.

The minimum required size is 104857600 bytes (100MB).\
Default: 524288000 (500MB)

```php
define('DOCKET_CACHE_MAXSIZE_DISK', 524288000);
```

## DOCKET\_CACHE\_MAXFILE

Set the maximum cache file can be store on disk.

Only numbers between 200 and 1000000 are allowed.\
Default: 50000

```php
define('DOCKET_CACHE_MAXFILE', 50000);
```

## DOCKET\_CACHE\_CHUNKCACHEDIR

Set to `true` to enable chunking cache files into smaller directories to avoid an excessive number of cache files in one directory.

Only enable it if you have difficulty clearing the cache manually or experience slowdowns when the cache becomes too large.\
Default:

```php
define('DOCKET_CACHE_CHUNKCACHEDIR', false);
```

## DOCKET\_CACHE\_MAXFILE\_LIVECHECK

Set to `true` to allow Docket Cache to monitor the cache file limit in real-time.\
Default:

```php
define('DOCKET_CACHE_MAXFILE_LIVECHECK', false);
```

## DOCKET\_CACHE\_EMPTYCACHE\_IGNORE

Set to `true` to enable excluding empty caches from being stored on disk.

Only enable it if you have an issue with inode/file limits.\
Default:

```php
define('DOCKET_CACHE_EMPTYCACHE_IGNORE', false);
```

## DOCKET\_CACHE\_PATH

Set the cache directory.\
Default:

```php
define('DOCKET_CACHE_PATH', WP_CONTENT_DIR.'/cache/docket-cache');
```

## DOCKET\_CACHE\_DATA\_PATH

Set the configuration directory.\
Default:

```php
define('DOCKET_CACHE_DATA_PATH', WP_CONTENT_DIR.'/docket-cache-data');
```

## DOCKET\_CACHE\_CONTENT\_PATH

Set the Docket Cache writable directory.\
Default:

```php
define('DOCKET_CACHE_CONTENT_PATH', WP_CONTENT_DIR);
```

By default, Docket Cache requires writable permission on WordPress **wp-content** directory for internal use. Defining this constant also change the default path for cache, configuration and object-cache Drop-In. The content path must exist and has proper permission. The `object-cache.php` Drop-In file needs to symlink with WordPress `wp-content/object-cache.php` or replace with wrapper file.

Please refer to the PHP [`open_basedir` ](https://www.php.net/manual/en/ini.core.php#ini.open-basedir)setting before set this constant.

#### Example:

```php
$ sudo mkdir -p /opt/dc-content
$ sudo chown apache:apache /opt/dc-content
$ sumod chmod 755 /opt/dc-content
$ sudo ln -s /opt/dc-content/object-cache.php /your-wp-path/wp-content/object-cache.php
```

#### Wrapper file:

```php
<?php
if (!\defined('ABSPATH')) {
    return;
}

if (!\defined('DOCKET_CACHE_CONTENT_PATH')) {
    return;
}

if ( !@is_file(DOCKET_CACHE_CONTENT_PATH.'/object-cache.php') ) {
    return;
}

@include_once DOCKET_CACHE_CONTENT_PATH.'/object-cache.php';
```

## DOCKET\_CACHE\_FLUSH\_DELETE

By default Docket Cache only empty the cache file when expire. Set to true to delete the cache file instead of truncate.\
Default:

```php
define('DOCKET_CACHE_FLUSH_DELETE', false);
```

## DOCKET\_CACHE\_FLUSH\_STALECACHE

Set to `true` to allow Garbage Collector (GC) immediately remove the stale cache abandoned by WordPress, WooCommerce and others after doing cache invalidation.\
Default:

```php
define('DOCKET_CACHE_FLUSH_STALECACHE', false);
```

## DOCKET\_CACHE\_STALECACHE\_IGNORE

Set to `true` to enable excluding stale cache created by WordPress, WooCommerce, and others from being stored on disk.\
Default:

```php
define('DOCKET_CACHE_STALECACHE_IGNORE', false);
```

Only enable it if you have an issue with inode/file limits.

## DOCKET\_CACHE\_GLOBAL\_GROUPS

Set the lists of groups cached at the network level in a Multisite setup.\
Default:

```php
define('DOCKET_CACHE_GLOBAL_GROUPS',
  [
    'blog-details',
    'blog-id-cache',
    'blog-lookup',
    'global-posts',
    'networks',
    'rss',
    'sites',
    'site-details',
    'site-lookup',
    'site-options',
    'site-transient',
    'users',
    'useremail',
    'userlogins',
    'usermeta',
    'user_meta',
    'userslugs'
  ]
);
```

## DOCKET\_CACHE\_IGNORED\_GROUPS

List of cache groups that should not be cached.\
Default:

```php
define('DOCKET_CACHE_IGNORED_GROUPS',
  [
    'counts',
    'plugins',
    'themes'
  ]
);
```

## DOCKET\_CACHE\_IGNORED\_KEYS

List of cache keys that should not be cached.\
Default: not set.

#### Example:

```php
define('DOCKET_CACHE_IGNORED_KEYS',['key1', 'key2']);
```

## DOCKET\_CACHE\_IGNORED\_GROUPKEY

List of cache groups and keys that should not be cached.\
Default: not set.

```php
define('DOCKET_CACHE_IGNORED_GROUPKEY',
  [
    'group1' => ['key1', 'key2'],
    'group2', => ['key1', 'key2']
  ]
);
```

## DOCKET\_CACHE\_LOG

Set to `true` or `false` to enable or disable cache log.\
Default:

```php
define('DOCKET_CACHE_LOG', false);
```

{% hint style="info" %}
The cache log intended to provide information on how the cache works. For performance and security concerns, deactivate if no longer needed.
{% endhint %}

## DOCKET\_CACHE\_LOG\_FILE

Set the log file.\
Default:

```php
define('DOCKET_CACHE_LOG_FILE', WP_CONTENT_DIR.'/.object-cache.log');
```

## DOCKET\_CACHE\_LOG\_TIME

Set the log time format when viewing. Available options utc, local, wp.\
Default:

```php
define('DOCKET_CACHE_LOG_TIME', 'utc');
```

## DOCKET\_CACHE\_LOG\_FLUSH

Set to `true` to empty the log file when the object cache is flushed.\
Default:

```php
define('DOCKET_CACHE_LOG_FLUSH', true);
```

## DOCKET\_CACHE\_LOG\_SIZE

Set the maximum size of the log file in bytes.\
Default: 10485760 (10MB)

```php
define('DOCKET_CACHE_LOG_SIZE', 10485760);
```

## DOCKET\_CACHE\_LOG\_ALL

By default, Docket Cache excludes it own process if WP\_DEBUG not defined as true.

Set to `true` or `false` to enable or disable to log all caches.\
Default:

```php
define('DOCKET_CACHE_LOG_ALL', false);
```

## DOCKET\_CACHE\_ADVCPOST

Set to `true` to enable Advanced Post Cache features that cache WP Queries for a post which results in faster data retrieval and reduced database workload.\
Default:

```php
define('DOCKET_CACHE_ADVCPOST', true);
```

{% hint style="info" %}
Since version 22.07.04, the Advanced Post Cache feature is only available for WordPress version 6.1 and below. Since it is already implemented in WordPress Core as WP\_Query caching.
{% endhint %}

## DOCKET\_CACHE\_ADVCPOSTTYPE

List of Post Types allowed for Advanced Post Cache.\
Default:

```php
define('DOCKET_CACHE_ADVCPOSTTYPE',
    [
     	'post',
        'page',
        'attachment',
        'revision',
        'nav_menu_item',
        'custom_css',
        'customize_changeset',
        'oembed_cache',
        'user_request',
        'wp_block',
        'wp_template',
        'wp_template_part',
        'wp_global_styles',
        'wp_navigation',
    ]
);
```

{% hint style="info" %}
Since version 22.07.04, this constant only works for WordPress version 6.1 and below.
{% endhint %}

## DOCKET\_CACHE\_ADVCPOSTTYPE\_ALL

Set to true to allow all Post Types for Advanced Post Cache.\
Default:

```php
define('DOCKET_CACHE_ADVCPOSTTYPE_ALL', false);
```

{% hint style="info" %}
Since version 22.07.04, this constant only works for WordPress version 6.1 and below.
{% endhint %}

## DOCKET\_CACHE\_CRONOPTMZDB

Enable Database Tables optimization.

Available options: never, daily, weekly, monthly.\
Default:

```php
define('DOCKET_CACHE_CRONOPTMZDB', 'never');
```

## DOCKET\_CACHE\_WPOPTALOAD

Set to `true` or `false` to enable or disable Suspend WP Options Autoload features.\
Default:

```php
define('DOCKET_CACHE_WPOPTALOAD', false);
```

## DOCKET\_CACHE\_MISC\_TWEAKS

Set to `true` or `false` to enable or disable miscellaneous WordPress performance tweaks.\
Default:

```php
define('DOCKET_CACHE_MISC_TWEAKS', true);
```

## DOCKET\_CACHE\_WOOTWEAKS

Set to `true` or `false` to enable or disable miscellaneous WooCommerce tweaks.\
Default:

```php
define('DOCKET_CACHE_WOOTWEAKS', true);
```

## DOCKET\_CACHE\_WOOADMINOFF

WooCommerce Admin or Analytics page is a new JavaScript-driven interface for managing stores.

Set to true to disable WooCommerce Admin feature-related.\
Default:

```php
define('DOCKET_CACHE_WOOADMINOFF', false);
```

## DOCKET\_CACHE\_WOOWIDGETOFF

Set to true to disable WooCommerce Classic Widget feature.\
Default:

```php
define('DOCKET_CACHE_WOOWIDGETOFF', false);
```

## DOCKET\_CACHE\_WOOWPDASHBOARDOFF

Set to true to disable WooCommerce meta box in the WordPress Dashboard.\
Default:

```php
define('DOCKET_CACHE_WOOWPDASHBOARDOFF', false);
```

## DOCKET\_CACHE\_WOOCARTFRAGSOFF

Set to true to disable WooCommerce Cart Fragments feature.\
Default:

```php
define('DOCKET_CACHE_WOOCARTFRAGSOFF', false);
```

## DOCKET\_CACHE\_WOOADDTOCHARTCRAWLING

Set to true to enable prevent robots crawling add-to-cart links.\
Default:

```php
define('DOCKET_CACHE_WOOADDTOCHARTCRAWLING', true);
```

## DOCKET\_CACHE\_WOOEXTENSIONPAGEOFF

Set to true to disable WooCommerce Extensions Page feature.\
Default:

```php
define('DOCKET_CACHE_WOOEXTENSIONPAGEOFF', true);
```

## DOCKET\_CACHE\_POSTMISSEDSCHEDULE

Set to `true` or `false` to enable or disable Post Missed Schedule Tweaks features.\
Default:

```php
define('DOCKET_CACHE_POSTMISSEDSCHEDULE', false);
```

## DOCKET\_CACHE\_OPTERMCOUNT

Set to `true` or `false` to enable or disable Term Count Queries optimization features.\
Default:

```php
define('DOCKET_CACHE_OPTERMCOUNT', true);
```

## DOCKET\_CACHE\_OPTWPQUERY

Set to `true` to enable WordPress Core Query optimization features. Docket Cache will attempt to optimize WordPress core query when enabled.\
Default:

```php
define('DOCKET_CACHE_OPTWPQUERY', true);
```

## DOCKET\_CACHE\_LIMITBULKEDIT

Set to `true` or `false` to enable or disable the Bulk Edit Actions when reaching the listed item limit.\
Default:

```php
define('DOCKET_CACHE_LIMITBULKEDIT', false);
```

## DOCKET\_CACHE\_LIMITBULKEDIT\_LIMIT

Set a limit of items listed for Bulk Edit Actions.\
Default:

```php
define('DOCKET_CACHE_LIMITBULKEDIT_LIMIT', 100);
```

## DOCKET\_CACHE\_MOCACHE

Set to `true` to enable WordPress Translation Caching features that improve the performance of the Translation function.\
Default:

```php
define('DOCKET_CACHE_MOCACHE', false);
```

## DOCKET\_CACHE\_MENUCACHE

Set to true to enable WordPress Menu Caching features that improve the performance of the WordPress menus generation.\
Default:

```php
define('DOCKET_CACHE_MENUCACHE', false);
```

## DOCKET\_CACHE\_MENUCACHE\_TTL

Default Menu Cache lifespan in seconds.

Only numbers between 86400 and 2419200 are allowed.\
Default: 1209600 (14 days)

```php
define('DOCKET_CACHE_MENUCACHE_TTL', 1209600);
```

## DOCKET\_CACHE\_SIGNATURE

Set to `true` or `false` to enable or disable Docket Cache signature at HTML footer and Server Header.\
Default:

```php
define('DOCKET_CACHE_SIGNATURE', true);
```

## DOCKET\_CACHE\_PRECACHE

Set to `true` to enable Object Cache Precaching features that increase cache performance by early loading cached objects based on the current URL.\
Default:

```php
define('DOCKET_CACHE_PRECACHE', true);
```

## DOCKET\_CACHE\_PRECACHE\_MAXFILE

Set the maximum precache file can be store on disk.

Only numbers between 100 and 1000000 are allowed.\
Default: 100

```php
define('DOCKET_CACHE_PRECACHE_MAXFILE', 100);
```

## DOCKET\_CACHE\_PRECACHE\_MAXKEY

Set the maximum precache keys.\
Default: 20

```php
define('DOCKET_CACHE_PRECACHE_MAXKEYe', 20);
```

## DOCKET\_CACHE\_PRECACHE\_MAXGROUP

Set the maximum precache groups.\
Default: 20

```php
define('DOCKET_CACHE_PRECACHE_MAXGROUP', 20);
```

## DOCKET\_CACHE\_IGNORED\_PRECACHE

List of cache groups and keys that should not be precached.\
Default:

```php
define('DOCKET_CACHE_IGNORED_PRECACHE',
    [
     	'freemius' => 'fs_accounts',
        'options' => [
            'uninstall_plugins',
            'auto_update_plugins',
            'active_plugins',
            'cron',
            'litespeed_messages',
            'litespeed.admin_display.messages',
        ],
	'site-options' => [
            '1:auto_update_plugins',
            '1:active_sitewide_plugins',
        ],
    ]
);
```

## DOCKET\_CACHE\_PRELOAD

Set to `true` or `false` to enable or disable cache preloading. If set to `true`, this plugin will fetch predefined URL related to the admin page.

The preload only runs when doing a cache flush.\
Default:

```php
define('DOCKET_CACHE_PRELOAD', false);
```

## DOCKET\_CACHE\_PAGELOADER

Set to `true` or `false` to enable or disable Admin Page Loader features.\
Default:

```php
define('DOCKET_CACHE_PAGELOADER', true);
```

## DOCKET\_CACHE\_TRANSIENTDB

By default WordPress stores Transients Cache in Database. When a persistent object cache is available, it switches Transients from Database to the object cache.

Some plugins use Transients not in the right way, they store too big data without expiration time. Without expiration, WordPress will place the Transient in the "alloptions" variable. It will make persistent object caching solutions like Docket Cache unable to handle it.

Set to `true` or `false` to enable or disable retaining Transients in the Database.\
Default:

```php
define('DOCKET_CACHE_TRANSIENTDB', false);
```

## DOCKET\_CACHE\_IGNORED\_TRANSIENTDB

A list of Transient names is excluded from being stored in the Database.\
Default:

```php
define('DOCKET_CACHE_IGNORED_TRANSIENTDB',
    [
        'doing_cron',
        'update_plugins',
        'update_themes',
        'update_core',
    ]
);
```

## DOCKET\_CACHE\_CRONBOT

The Cronbot is an [external service](https://cronbot.docketcache.com/) that pings your website every hour to keep WordPress Cron running actively. Only site Timezone, URL and version are involved when enabling this service.

Set to `true` or `false` to enable or disable Cronbot Service.\
Default:

```php
define('DOCKET_CACHE_CRONBOT', false);
```

## DOCKET\_CACHE\_CRONBOT\_MAX

Maximum sites allowed in Multisite setup.\
Default:

```php
define('DOCKET_CACHE_CRONBOT_MAX', 10);
```

## DOCKET\_CACHE\_OPCVIEWER

Set to true or false to enable or disable the OPcache viewer feature.\
Default:

```php
define('DOCKET_CACHE_OPCVIEWER', false);
```

## DOCKET\_CACHE\_GCACTION

Set to `true` to enable Docket Cache Garbage Collector action button at Overview screen.\
Default:

```php
define('DOCKET_CACHE_GCACTION', false);
```

## DOCKET\_CACHE\_FLUSHACTION

Set to `true` to enable Docket Cache the additional Flush Cache action button on the Configuration screen.\
Default:

```php
define('DOCKET_CACHE_FLUSHACTION', false);
```

## DOCKET\_CACHE\_AUTOUPDATE

Set to `true` or `false` to force enable or disable automatic updates of the Docket Cache.\
Default:

```php
define('DOCKET_CACHE_AUTOUPDATE', true);
```

## DOCKET\_CACHE\_CHECKVERSION

The Check Version allows Docket Cache to check any critical future version that requires removing cache files before doing the updates, purposely to avoid error-prone.

Set to `true` or `false` to enable or disable critical version checking.\
Default:

```php
define('DOCKET_CACHE_CHECKVERSION', true);
```

## DOCKET\_CACHE\_STATS

Set to `true` or `false` to enable or disable object cache data stats at Overview screen.\
Default:

```php
define('DOCKET_CACHE_STATS', true);
```

## DOCKET\_CACHE\_PINGBACK

Set to true to disable WordPress XML-RPC and Pingbacks related features.\
Default:

```php
define('DOCKET_CACHE_PINGBACK', false);
```

## DOCKET\_CACHE\_HEADERJUNK

Set to true to disable WordPress features related to HTML header such as meta generators and feed links to reduce the page size.\
Default:

```php
define('DOCKET_CACHE_HEADERJUNK', false);
```

## DOCKET\_CACHE\_WPEMOJI

Set to true to disable WordPress Emoji feature.\
Default:

```php
define('DOCKET_CACHE_WPEMOJI', false);
```

## DOCKET\_CACHE\_WPFEED

Set to true to disable WordPress Feed feature.\
Default:

```php
define('DOCKET_CACHE_WPFEED', false);
```

## DOCKET\_CACHE\_WPEMBED

Set to true to disable WordPress Embed feature.\
Default:

```php
define('DOCKET_CACHE_WPEMBED', false);
```

## DOCKET\_CACHE\_WPLAZYLOAD

Set to true to disable WordPress Lazy Load feature.\
Default:

```php
define('DOCKET_CACHE_WPLAZYLOAD', false);
```

## DOCKET\_CACHE\_WPSITEMAP

Set to true to disable WordPress Auto-Sitemap feature.\
Default:

```php
define('DOCKET_CACHE_WPSITEMAP', false);
```

## DOCKET\_CACHE\_WPAPPPASSWORD

Set to true to disable WordPress Application Passwords feature.\
Default:

```php
define('DOCKET_CACHE_WPAPPPASSWORD', false);
```

## DOCKET\_CACHE\_WPDASHBOARDNEWS

Set to true to disable WordPress Events & News Feed at Dashboard.\
Default:

```php
define('DOCKET_CACHE_WPDASHBOARDNEWS', false);
```

## DOCKET\_CACHE\_WPBROWSEHAPPY

Set to true to disable the WordPress Browse Happy HTTP API requests, which checks whether the user needs a browser update.\
Default:

```php
define('DOCKET_CACHE_WPBROWSEHAPPY', false);
```

## DOCKET\_CACHE\_WPSERVEHAPPY

Set to true to disable the WordPress Serve Happy HTTP API request, which checks whether the user needs to update PHP.\
Default:

```php
define('DOCKET_CACHE_WPSERVEHAPPY', false);
```

## DOCKET\_CACHE\_POSTVIAEMAIL

Set to true to disable the WordPress post-via-email functionality.\
Default:

```php
define('DOCKET_CACHE_POSTVIAEMAIL', false);
```

## DOCKET\_CACHE\_LIMITHTTPREQUEST

Set to true to limit HTTP requests in WP-Admin.

This option will block any HTTP requests made by plugins or themes that used `wp_remote_post`, `wp_remote_get` or `wp_remote_request` functions that are not invoked in standard WP-Admin pages like Post, Pages, Plugins, Media and others.

Default:

```php
define('DOCKET_CACHE_LIMITHTTPREQUEST', false);
```

## DOCKET\_CACHE\_LIMITHTTPREQUEST\_WHITELIST

Set the list of hosts excluded from Limit HTTP requests options.\
Default:

```php
define('DOCKET_CACHE_LIMITHTTPREQUEST_WHITELIST', []);
```

#### Example:

```php
define('DOCKET_CACHE_LIMITHTTPREQUEST_WHITELIST', 
    [
        'feeds.feedburner.com',
        'api.docketcache.com'
    ]
);
```

## DOCKET\_CACHE\_GCRON\_DISABLED

Set to true to disable Garbage Collector Cron Events. By defining it as true, Docket Cache will not install Cron Event for Garbage Collector. You need to run it manually using wp-cli or a custom Cron Events.\
Default:

```php
define('DOCKET_CACHE_GCRON_DISABLED', false);
```

#### Example for WP-CLI:

```
wp cache run:gc
```

#### Example for custom Cron Events:

```php
<?php
// Place this code in wp-content/mu-plugins/docketcache-gcron.php
if ( !defined('DOCKET_CACHE_GCRON_DISABLED') || !DOCKET_CACHE_GCRON_DISABLED) {
    exit;
}

add_action('docketcache_custom_gcron', function() {
    if ( has_filter('docketcache/filter/garbagecollector') ) {
        $results = apply_filters('docketcache/filter/garbagecollector', true);
        if (!empty($results) && \is_object($results)) {
            if ($results->is_locked) {
                // Process is locked.
                return;
            }
            // Process is Ok.
        }
    }
});

if (!wp_next_scheduled('docketcache_custom_gcron')) {
    wp_schedule_event(time(), 'hourly', 'docketcache_custom_gcron');
}

```

## DOCKET\_CACHE\_DISABLED

Set to true to disable the Docket Cache object cache feature at runtime. By defining it as true, Docket Cache will ignore to install and uninstall the Drop-in file.\
Default:

```php
define('DOCKET_CACHE_DISABLED', false);
```
