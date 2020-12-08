---
description: The command line interface for WordPress.
---

# WP-CLI

`Updated: 08-Dec-2020 | Version: >= 20.11.01`

WP-CLI is the official command-line interface for WordPress. The Docket Cache extends default `wp cache` command with additional sub-commands.

The following command supported.

## wp cache status

Display the Docket Cache status.

```text
wp cache status
```

**Example output:**

```text
Cache Status   : Enabled
Cache Path     : /wp-content/cache/docket-cache
Cache Size     : 3M
```

## wp cache dropin:enable

Enable the Docket Cache Drop-In file. The default behaviour is to create the object cache Drop-In and replace any object cache Drop-In is present.

```text
wp cache dropin:enable
```

## wp cache dropin:disable

Disable the Docket Cache Drop-In file. The default behaviour is to delete the object cache Drop-In unless an unknown object cache Drop-In is present.

```text
wp cache dropin:disable
```

## wp cache dropin:update

Update the Docket Cache Drop-In file. The default behaviour is to overwrite any existing object cache Drop-In.

```text
wp cache update
```

## wp cache flush

Remove the cache files.

```text
wp cache flush
```

## wp cache flush:precache

Remove the precache files.

```text
wp cache flush:precache
```

## wp cache reset:lock

Reset the Docket Cache lock files.

```text
wp cache reset:lock
```

## wp cache reset:cron

Reset the Docket Cache cron event.

```text
wp cache reset:cron
```

**Example output:**

```text
Resetting cron event. Please wait..
+------------------------------------+---------------------+-----------------------+------------+
| hook                               | next_run_gmt        | next_run_relative     | recurrence |
+------------------------------------+---------------------+-----------------------+------------+
| docketcache_gc                     | 2020-12-08 16:58:36 | now                   | 5 minutes  |
| docketcache_watchproc              | 2020-12-08 16:58:36 | now                   | 1 hour     |
| docketcache_checkversion           | 2020-12-08 16:58:36 | now                   | 5 days     |
+------------------------------------+---------------------+-----------------------+------------+
Success: Cron event has been reset.

```

## wp cache run:gc

Run the Docket Cache garbage collector \(GC\).

```text
wp cache run:gc
```

**Example output:**

```text
Executing the garbage collector. Please wait..
-----------------------------------:----------
Cache MaxTTL                       : 345600
Cache File Limit                   : 50000
Cache Disk Limit                   : 500M
-----------------------------------:----------
Cleanup Cache MaxTTL               : 0
Cleanup Cache File Limit           : 0
Cleanup Cache Disk Limit           : 0
-----------------------------------:----------
Total Cache Cleanup                : 0
Total Cache Ignored                : 0
Total Cache File                   : 1580
-----------------------------------:----------
Success: Executing the garbage collector completed.
```

## wp cache run:cron

Run all cron event.

```text
wp cache run:cron
```

**Example output:**

```text
Executing the cron event. Please wait..
Executed the cron event 'docketcache_watchproc' in 0.011s.
Executed the cron event 'docketcache_gc' in 0.338s.
Executed the cron event 'wp_privacy_delete_old_export_files' in 0.011s.
Executed the cron event 'wp_version_check' in 6.804s.
Executed the cron event 'wp_update_plugins' in 2.817s.
Executed the cron event 'wp_update_themes' in 0.011s.
Executed the cron event 'recovery_mode_clean_expired_keys' in 0.005s.
Executed the cron event 'wp_scheduled_delete' in 0.005s.
Executed the cron event 'delete_expired_transients' in 0.005s.
Executed the cron event 'wp_scheduled_auto_draft_delete' in 0.007s.
Executed the cron event 'docketcache_checkversion' in 0.012s.
Executed the cron event 'wp_site_health_scheduled_check' in 0.091s.
Success: Executed a total of 12 cron events.
```

## wp cache run:stats

Run the Docket Cache stats function to collect cache data.

```text
wp cache run:stats
```

**Example output:**

```text
Executing the cache stats. Please wait..
Object size    : 3M
File size      : 4M
Total file     : 1578
Success: Executing the cache stats completed.
```



