---
hidden: true
---

# Magento 2 Hosting Reference Guide

We provide optimized server environments for running Magento 2. As the hosting provider, we maintain the server and infrastructure, but Magento configuration, upgrades, and customization are your responsibility.

This guide explains where Magento 2 is installed, how to use the CLI, and manage permissions.

## File Structure

Your Magento 2 installation resides in the following directories:

| Path                   | Purpose                                                              |
| ---------------------- | -------------------------------------------------------------------- |
| `~/public_html`        | Magento 2 project root and document root. Run all CLI commands here. |
| `~/public_html/bin`    | Contains Magento CLI scripts (e.g., `magento`).                      |
| `~/public_html/vendor` | Composer-managed PHP dependencies.                                   |
| `~/public_html/app`    | Magento’s core and custom modules/themes.                            |
| `~/public_html/var`    | Cache, generated code, sessions, and logs.                           |
| `~/public_html/pub`    | Web-accessible static files (HTML, CSS, JS, media).                  |
| `~/nginx`              | Custom Nginx configurations.                                         |
| `~/nginx/mage.runmaps` | Runmaps for configuring domain → store mappings.                     |

## Magento CLI Commands

Run all Magento CLI commands from the `~/public_html` directory:

```bash
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy
php bin/magento cache:flush
...
```

These commands are commonly used for module installation, compilation, and deployment of static assets.

## Installing Composer Dependencies

Magento uses Composer to manage PHP dependencies. To install them:

```bash
cd ~/public_html
composer install
```

Make sure your hosting plan uses a supported PHP version and has all required extensions.

## Permissions

Magento requires specific permissions to function correctly. Run the following to ensure proper access:

```bash
find var generated vendor pub/static pub/media app/etc -type f -exec chmod 644 {} \;
find var generated vendor pub/static pub/media app/etc -type d -exec chmod 755 {} \;
chown -R $USER:$USER ~/public_html
```

This ensures files are readable and directories are executable by the web server.

## Caching

Magento supports various caching backends. By default, it uses the filesystem, but we highly recommend using Redis and Varnish!

To clear Magento's internal caches:

```bash
php bin/magento cache:clean
php bin/magento cache:flush
```

To flush Varnish and Redis caches respectively (if enabled in your environment):

```bash
tscli varnish clear
tscli redis clear
```

---

If you need a separate staging server, additional PHP modules, or caching setup, please contact our support team.

