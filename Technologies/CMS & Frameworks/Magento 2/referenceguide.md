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

A complete list of Magento CLI commands can be found [here](magento2cli.md)

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

### Configuring Redis
You can find our documentation on how to set up Redis [here](../../Caching/redis.md)

### Clearing the cache


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

## Message Queue Consumer Management

Magento uses message queues for async tasks. You can manage its consumers with `systemd`.

**Path:** `~/.config/systemd/user/magento-consumer@.service`

```ini
[Unit]
Description=Magento Consumer (%i)
After=network-online.target
Requires=dbus.socket
StartLimitIntervalSec=0

[Service]
Type=simple
WorkingDirectory=%h/public_html
ExecStart=php %h/public_html/bin/magento queue:consumers:start %I --single-thread --max-messages=10000
RestartSec=10s
Restart=always

[Install]
WantedBy=default.target
```

### General Information

- `%i` is a placeholder for the consumer name—reusable template
- `%h` resolves to the user’s home directory
- `--single-thread` ensures one thread per process
- `--max-messages=10000` helps restart periodically to prevent memory issues

**Examples:**

```bash
systemctl --user status magento-consumer@sales.rule.update.coupon.usage.service
systemctl --user status magento-consumer@product_action_attribute.update.service
```

You can repeat this for any other queue. Each one runs in its own isolated service.

---

If you need a separate staging server, additional PHP modules, or caching setup, please contact our support team.

