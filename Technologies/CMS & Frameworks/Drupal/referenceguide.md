---
hidden: true
---

# Drupal Hosting Reference Guide

We provide optimized server environments for running Drupal. As the
hosting provider, we maintain the server and infrastructure, but Drupal
configuration, upgrades, and customization are your responsibility.

This guide explains where Drupal is installed on TurboStack, how to use the CLI, and
how to manage permissions.

## File Structure

Your Drupal installation resides in the following directories:

  -----------------------------------------------------------------------------------------
  Path                                  Purpose
  ------------------------------------- ---------------------------------------------------
  `~/public_html`                       Drupal project root and document root. Run all CLI
                                        commands here.

  `~/public_html/core`                  Drupal core framework files.

  `~/public_html/modules`               Contributed and custom Drupal modules.

  `~/public_html/themes`                Installed Drupal themes.

  `~/public_html/profiles`              Installation profiles used when installing Drupal.

  `~/public_html/sites`                 Site-specific configuration, files, and settings.

  `~/public_html/sites/default/files`   Public file uploads and generated assets.

  `~/public_html/vendor`                Composer-managed PHP dependencies.

  `~/nginx`                             Custom Nginx configurations for your site.
  -----------------------------------------------------------------------------------------

> Note: In multi-site configurations, additional directories may exist
> under `~/public_html/sites/`.

## Drupal CLI (Drush)

Drupal is typically managed using **Drush**, its command-line interface.

Run all Drush commands from the `~/public_html` directory:

``` bash
cd ~/public_html
vendor/bin/drush status
```

Common Drush commands include:

| Command | Purpose |
|--------|---------|
| `drush cr` | Rebuild Drupal caches. |
| `drush updb` | Run database updates after module/core upgrades. |
| `drush cim` | Import configuration changes from config files. |
| `drush cex` | Export active configuration to files. |
| `drush pm:list` | List installed modules. |

If Drush is installed globally on your system, you may run `drush`
directly. Otherwise use:

``` bash
vendor/bin/drush
```

## Installing Composer Dependencies

Drupal uses Composer to manage PHP dependencies and modules.

To install dependencies:

``` bash
cd ~/public_html
composer install
```

To update dependencies:

``` bash
composer update
```

Make sure your hosting plan uses a supported PHP version and has all
required extensions for Drupal.

## Permissions

Drupal requires proper permissions for the file upload directory.

Run the following to ensure correct access:

``` bash
find ~/public_html -type f -exec chmod 644 {} \;
find ~/public_html -type d -exec chmod 755 {} \;
chmod -R 775 ~/public_html/sites/default/files
chown -R $USER:$USER ~/public_html
```

This ensures:

-   Files are readable by the web server
-   Directories are accessible
-   The `files` directory is writable for uploads and generated assets

## Caching

Drupal includes an internal caching system. Clearing caches is commonly
required after installing modules or deploying configuration.

### Clearing Drupal Cache

Use Drush:

``` bash
vendor/bin/drush cr
```

### Redis and Varnish Caching

For improved performance, we recommend using Redis and optionally a
reverse proxy such as Varnish. Both these options can be easily configured in the TurboStack App.

While Varnish works out-of-the-box, Redis needs to be specifically configured in your application. You can find our documentation on how to configure Redis caching
[here](../../Caching/redis.md)

More info on Varnish can be found [here](../../Caching/varnish.md)

### Clearing Redis and Varnish Caches

If enabled in your environment:

``` bash
tscli redis clear
tscli varnish clear
```

These commands flush Redis and Varnish caches respectively.

## Running Scheduled Tasks (Cron)

Drupal relies on cron jobs to perform periodic tasks such as indexing,
sending emails, and cleaning logs.

Run cron manually with:

``` bash
vendor/bin/drush cron
```

You may configure a user cron job if needed:

``` bash
crontab -e
```

Example:

``` bash
*/15 * * * * cd ~/public_html && vendor/bin/drush cron
```

This example runs Drupal cron every 15 minutes.

## Nginx Configuration

You can place custom Nginx configuration files in:

    ~/nginx

These configurations allow you to customize routing, caching behavior,
or security headers for your Drupal installation.

Changes to Nginx configuration may require a reload by our
infrastructure, which can be done with the `tscli nginx reload' command.

More information on Nginx configuration can be found [here](../../Web%20Servers/NGINX)

------------------------------------------------------------------------

If you need a separate staging server, additional PHP modules, or
caching setup, please contact our support team!
