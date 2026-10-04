---
description: Docket Cache installation methods.
---

# Installation

## Requirements

To use Docket Cache requires minimum:

* PHP 7.2.5
* WordPress 5.4

We also recommend:

* Zend OPcache

Docket Cache saves the cache as PHP files, so it works with or without OPcache. With OPcache, PHP keeps those files ready in memory and does not have to read them from disk on every page load, which makes the cache much faster.

## WordPress Plugin

1. In your WordPress admin click `Plugins -> Add New` **.**
2. Search plugins **Docket Cache** and click `Install Now`.
3. Click `Activate` or `Network Activate` in Multisite setups.
4. Click **Docket Cache** in the left menu to access the admin interface.

{% hint style="info" %}
Please wait around 5 seconds for Docket Cache ready to cache the objects.
{% endhint %}

## Manual Installation

1. Download the plugin as a [ZIP file](https://github.com/nawawi/docket-cache/archive/master.zip) from GitHub or from [WordPress Plugin Directory](https://wordpress.org/plugins/docket-cache/).
2. In your WordPress admin click `Plugins -> Add New -> Upload Plugin`.
3. Upload the ZIP file and Activate the plugin.
4. Click Activate or Network Activate in Multisite setups.
5. Click Docket Cache in the left menu to access the admin page.

## Via WP-CLI

[`WP-CLI`](http://wp-cli.org) is the official command-line interface for WordPress. You can install Docket Cache using the `wp` command like this:

```
wp plugin install docket-cache --activate
```
