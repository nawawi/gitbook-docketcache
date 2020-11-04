---
description: Docket Cache uses constants variable as main configuration methods.
---

# Configuration

Constants are like variables except that once they are defined they cannot be changed or undefined. To change the behaviour of Docket Cache, the following PHP constants can be defined in your `wp-config.php` file.

## DOCKET\_CACHE\_MAXTTL

Cache file lifespan in seconds. Only numbers between 86400 and 2419200 are accepted.  
Default: 4 days = 345600 seconds

```php
define('DOCKET_CACHE_MAXTTL', 345600);
```

## DOCKET\_CACHE\_MAXSIZE

Set the maximum size of an object can be cache in bytes. Maximum accepted size is 10485760 \(10MB\).  
Default: 3MB. 1MB = 1048576 bytes

```php
define('DOCKET_CACHE_MAXSIZE', 3145728);
```

## DOCKET\_CACHE\_MAXSIZE\_DISK

Set the maximum size of the cache storage on disk. Minimum accepted size is 104857600. \(100MB\).  
Default: 500MB = 524288000

```php
define('DOCKET_CACHE_MAXSIZE_DISK', 524288000);
```

## DOCKET\_CACHE\_MAXFILE

Cache maximum accelerated files. Only numbers between 200 and 200000 are accepted.  
Default: 50000

```php
define('DOCKET_CACHE_MAXFILE', 50000);
```

## DOCKET\_CACHE\_PATH

Set the cache directory.  
Default:

```php
define('DOCKET_CACHE_PATH', WP_CONTENT_DIR.'/cache/docket-cache');
```

## DOCKET\_CACHE\_DATA\_PATH

Set the configuration directory.  
Default:

```php
define('DOCKET_CACHE_DATA_PATH', WP_CONTENT_DIR.'/docket-cache-data/');
```

## DOCKET\_CACHE\_CONTENT\_PATH

Set the Docket Cache writable directory.   
Default:

```php
define('DOCKET_CACHE_CONTENT_PATH', WP_CONTENT_DIR);
```

By default, Docket Cache requires writable permission on WordPress **wp-content** directory for internal use. Defining this constant also change the default path for cache, configuration and object-cache Drop-In. The content path must exist and has proper permission. The `object-cache.php` Drop-In file needs to symlink with WordPress `wp-content/object-cache.php` or replace with wrapper file.

Please refer to PHP [open\_basedir](https://www.php.net/manual/en/ini.core.php#ini.open-basedir) setting before set this constant.

**Example:**

```php
$ sudo mkdir -p /opt/dc-content
$ sudo chown apache:apache /opt/dc-content
$ sumod chmod 755 /opt/dc-content
$ sudo ln -s /opt/dc-content/object-cache.php /your-wp-path/wp-content/object-cache.php
```

**Wrapper file:**

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

By default Docket Cache only empty the cache file. Set to `true` to delete the cache file when flushed.  
Default:

```php
define('DOCKET_CACHE_FLUSH_DELETE', false);
```

## DOCKET\_CACHE\_GLOBAL\_GROUPS

Lists of cache groups that share a cache with other sites in a Multisite setup.  
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

List of cache groups that should not be cached.  
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

## DOCKET\_CACHE\_LOG

Set to `true` or `false` to enable or disable cache log.  
Default:

```php
define('DOCKET_CACHE_LOG', false);
```

{% hint style="info" %}
The cache log intended to provide information on how the cache works. For performance and security concerns, deactivate if no longer needed.
{% endhint %}

## DOCKET\_CACHE\_LOG\_FILE

Set the log file.  
Default:

```php
define('DOCKET_CACHE_LOG_FILE', WP_CONTENT_DIR.'/.object-cache.log');
```

## DOCKET\_CACHE\_LOG\_TIME

Set the log time format when viewing. Available options utc, local, wp.  
Default:

```php
define('DOCKET_CACHE_LOG_TIME', 'utc');
```

## DOCKET\_CACHE\_LOG\_FLUSH

Set to `true` to empty the log file when the object cache is flushed.  
Default:

```php
define('DOCKET_CACHE_LOG_FLUSH', true);
```

## DOCKET\_CACHE\_LOG\_SIZE

Set the maximum size of the log file in bytes.  
Default: 10MB. 1MB = 1048576 bytes

```php
define('DOCKET_CACHE_LOG_SIZE', 10485760);
```

## DOCKET\_CACHE\_LOG\_ALL

By default, Docket Cache excludes it own process if WP\_DEBUG not defined as true. Set to `true` or `false` to enable or disable to log all caches.  
Default:

```php
define('DOCKET_CACHE_LOG_ALL', false);
```

## DOCKET\_CACHE\_ADVCPOST

Set to `true` or `false` to enable or disable Advanced Post Cache features.  
Default:

```php
define('DOCKET_CACHE_ADVCPOST', true);
```

## DOCKET\_CACHE\_CRONOPTMZDB

Enable Database Tables optimization. Available options: never, daily, weekly, monthly.  
Default:

```php
define('DOCKET_CACHE_CRONOPTMZDB', 'never');
```

## DOCKET\_CACHE\_WPOPTALOAD

Set to `true` or `false` to enable or disable Suspend WP Options Autoload features.  
Default:

```php
define('DOCKET_CACHE_WPOPTALOAD', false);
```

## DOCKET\_CACHE\_MISC\_TWEAKS

Set to `true` or `false` to enable or disable miscellaneous WordPress performance tweaks.  
Default:

```php
define('DOCKET_CACHE_MISC_TWEAKS', true);
```

## DOCKET\_CACHE\_WOOTWEAKS

Set to `true` or `false` to enable or disable miscellaneous WooCommerce tweaks.  
Default:

```php
define('DOCKET_CACHE_WOOTWEAKS', true);
```

## DOCKET\_CACHE\_POSTMISSEDSCHEDULE

Set to `true` or `false` to enable or disable Post Missed Schedule Tweaks features.  
Default:

```php
define('DOCKET_CACHE_POSTMISSEDSCHEDULE', false);
```

## DOCKET\_CACHE\_OPTERMCOUNT

Set to `true` or `false` to enable or disable Term Count Queries optimization features.  
Default:

```php
define('DOCKET_CACHE_OPTERMCOUNT', true);
```

## DOCKET\_CACHE\_OPTWPQUERY

Set to `true` or `false` to enable or disable WordPress Core Query optimization features.  
Default:

```php
define('DOCKET_CACHE_OPTWPQUERY', true);
```

## DOCKET\_CACHE\_MOCACHE

Set to `true` or `false` to enable or disable WordPress Translation Caching features.  
Default:

```php
define('DOCKET_CACHE_MOCACHE', false);
```

## DOCKET\_CACHE\_SIGNATURE

Set to `true` or `false` to enable or disable Docket Cache signature at HTML footer and Server Header.  
Default:

```php
define('DOCKET_CACHE_SIGNATURE', true);
```

## DOCKET\_CACHE\_PRECACHE

Set to `true` or `false` to enable or disable Object Cache Precaching features.  
Default:

```php
define('DOCKET_CACHE_PRECACHE', true);
```

## DOCKET\_CACHE\_PRELOAD

Set to `true` or `false` to enable or disable cache preloading. If set to `true`, this plugin will fetch predefined URL related to the admin page.  
Default:

```php
define('DOCKET_CACHE_PRELOAD', false);
```

## DOCKET\_CACHE\_PAGELOADER

Set to `true` or `false` to enable or disable Admin Page Loader features.  
Default:

```php
define('DOCKET_CACHE_PAGELOADER', true);
```

## DOCKET\_CACHE\_CRONBOT

The Cronbot is an [external service](https://cronbot.docketcache.com/) that pings your website every hour to keep WordPress Cron running actively. Only site Timezone, URL and version are involved when enabling this service.

Set to `true` or `false` to enable or disable Cronbot Service.  
Default:

```php
define('DOCKET_CACHE_CRONBOT', false);
```

## DOCKET\_CACHE\_CRONBOT\_MAX

Maximum sites allowed in Multisite setup.  
Default:

```php
define('DOCKET_CACHE_CRONBOT_MAX', 10);
```

## DOCKET\_CACHE\_AUTOUPDATE

Set to `true` or `false` to enable or disable Docket Cache auto-update.  
Default:

```php
define('DOCKET_CACHE_AUTOUPDATE', true);
```

## DOCKET\_CACHE\_CHECKVERSION

The Check Version allows Docket Cache to check any critical future version that requires removing cache files before doing the updates, purposely to avoid error-prone.

Set to `true` or `false` to enable or disable critical version checking.  
Default:

```php
define('DOCKET_CACHE_CHECKVERSION', true);
```

## DOCKET\_CACHE\_STATS

Set to `true` or `false` to enable or disable object cache data stats at Overview page.  
Default:

```php
define('DOCKET_CACHE_STATS', true);
```

## DOCKET\_CACHE\_DISABLED

Set to `true` to disable the object cache at runtime. No persistent cached objects at this time.  
Default:

```php
define('DOCKET_CACHE_DISABLED', false);
```

