---
hidden: true
---
# Shopware Hosting Reference Guide

## File Structure

Your Shopware project is expected to use `~/public_html` as the public web root. This must point to Shopware’s `/public` directory, either directly or via symlink. The same applies to the main Shopware directory — you may symlink it if you are using a release-based deployment.

| Path                 | Purpose                                                             |
| -------------------- | ------------------------------------------------------------------- |
| `~/public_html`      | Web document root — must point to the `public/` folder of Shopware. |
| `~/shopware/`        | Base directory of the Shopware installation. Can also be a symlink. |
| `~/shopware/public/` | Public assets and front controller (`index.php`).                   |
| `~/shopware/vendor/` | Composer dependencies.                                              |
| `~/shopware/custom/` | Custom plugins and themes.                                          |
| `~/shopware/config/` | Configuration files (`.env`, `packages/`, `services.yaml`, etc.).   |
| `~/nginx/`           | Optional: Custom Nginx configuration.                               |

> ℹ️ **Note:** Symbolic links are fully supported — e.g., `public_html` → `current/public`, `shopware` → `releases/20250715`. Just ensure that the final paths resolve correctly to the intended directories inside the Shopware project.

## Permissions

Correct file and directory permissions are required to allow Shopware to read/write necessary files while maintaining security:

```bash
find ~/shopware -type f -exec chmod 644 {} \;
find ~/shopware -type d -exec chmod 755 {} \;
chown -R $USER:$USER ~/shopware
```

Make sure your CLI user has ownership of the entire Shopware installation to avoid permission issues during updates or plugin installation.

## Composer

Composer is required to manage Shopware packages and dependencies. It should be used from the base directory of the Shopware installation:

```bash
cd ~/shopware
composer install
```

Use this for installing plugins, platform updates, or rebuilding autoloaders.

## Cache Management

Shopware includes multiple caching layers, which should be cleared when installing plugins, making configuration changes, or performing updates.

Clear cache via CLI:

```bash
cd ~/shopware
bin/console cache:clear
```

If Redis and Varnish are in use (recommended), you may also manually clear these services:

```bash
tscli redis clear
tscli varnish clear
```

## Cronjobs and Scheduled Tasks

Shopware handles background jobs and scheduled tasks internally. To ensure these run correctly, make sure the following cron is set up in your user crontab:

```bash
*/5 * * * * cd ~/shopware && bin/console scheduled-task:run >> ~/logs/scheduled-task.log 2>&1
```

## Updates

Before updating Shopware, always back up your code and database. Then:

```bash
cd ~/shopware
composer update
bin/console system:update:finish
```

!!! Important
To safely test new modules or updates, use a separate **staging server**. Do **not** run staging environments locally on the same production host.

Why Separate Servers?
* Prevents production disruption.
* Isolates tests and experiments.
* Maintains better performance and security.

We offer affordable and optimized staging environments. Contact support to request a staging clone of your production environment.
!!!

## Logs

Shopware logs are stored in:

```
~/shopware/var/log
```

You can monitor errors, system events, and plugin issues from these files.

---

For any issues related to server performance or access-level troubleshooting, please contact our support team. For application-specific questions or plugin support, refer to the Shopware documentation or community forums.
