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

## cache dropin:enable

Enables the Docket object cache. The default behaviour is to create the object cache drop-in and replace any object cache drop-in is present.

```text
wp cache dropin:enable
```

## cache dropin:disable

Disables the Docket object cache. The default behaviour is to delete the object cache drop-in unless an unknown object cache drop-in is present.

```text
wp cache dropin:disable
```

## cache dropin:update

Updates the Docket object cache drop-in. The default behaviour is to overwrite any existing object cache drop-in.

```text
wp cache update
```

## cache flush

Flushes the object cache.

```text
wp cache flush
```

## cache run:gc

Run garbage collector

```text
wp cache run:gc
```

## cache reset:lock

Remove all internal lock files

```text
wp cache reset:unlock
```



