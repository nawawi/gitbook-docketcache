---
description: The command line interface for WordPress.
---

# WP-CLI

`Updated: 29-Nov-2020 | Version: >= 20.09.01`

WP-CLI is the official command-line interface for WordPress. The Docket Cache extends default `wp cache` command with additional sub-commands.

The following command supported:

## cache status

Show the Docket object cache status.

```text
wp cache status
```

## cache enable

Enables the Docket object cache. The default behaviour is to create the object cache drop-in and replace any object cache drop-in is present.

```text
wp cache enable
```

## cache disable

Disables the Docket object cache. The default behaviour is to delete the object cache drop-in unless an unknown object cache drop-in is present.

```text
wp cache disable
```

## cache update

Updates the Docket object cache drop-in. The default behaviour is to overwrite any existing object cache drop-in.

```text
wp cache update
```

## cache flush

Flushes the object cache.

```text
wp cache flush
```

## cache gc

Run garbage collector

```text
wp cache gc
```

## cache unlock

Remove all internal lock files

```text
cache unlock
```



