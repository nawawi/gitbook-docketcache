---
description: The command line interface for WordPress.
---

# WP-CLI

`Updated: 08-Dec-2020 | Version: >= 20.11.01`

WP-CLI is the official command-line interface for WordPress. The Docket Cache extends default `wp cache` command with additional sub-commands.

The following command supported:

## wp cache status

Show the Docket object cache status.

```text
wp cache status
```

## wp cache dropin:enable

Enables the Docket object cache. The default behaviour is to create the object cache drop-in and replace any object cache drop-in is present.

```text
wp cache dropin:enable
```

## wp cache dropin:disable

Disables the Docket object cache. The default behaviour is to delete the object cache drop-in unless an unknown object cache drop-in is present.

```text
wp cache dropin:disable
```

## wp cache dropin:update

Updates the Docket object cache drop-in. The default behaviour is to overwrite any existing object cache drop-in.

```text
wp cache update
```

## wp cache flush

Flushes the object cache.

```text
wp cache flush
```

## wp cache run:gc

Run garbage collector

```text
wp cache run:gc
```

## wp cache reset:lock

Remove all internal lock files

```text
wp cache reset:unlock
```



