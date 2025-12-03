---
hidden: true
---
# WordPress Best Practices
Here you'll find some best practices and suggestions for WordPress, to get the most out of your TurboStack environment.

## Do NOT Update All Plugins
Updating all plugins blindly often leads to errors.  
Instead, use **Blackfire** to analyze performance issues and optimize selectively.

## Always Create a Real Cronjob
WordPress uses a built-in “pseudo-cron” system called wp-cron.php, but it has several weaknesses:
  - wp-cron is triggered on page load.
If your site has low traffic, cron tasks might run late or not at all.
  - If your site has high traffic, wp-cron may trigger too frequently, causing unnecessary load.
  - Every visit can trigger a separate php process to execute wp-cron. This means increased research usage.

### Disable wp-cron in wp-config.php
To disable wp-cron, add the following line to wp-config.php:
```
define('DISABLE_WP_CRON', 'true');
```

### Crontab
To start making cronjobs, you need to initialize it first with the following command:
```
crontab -e
```
If this is the first time you are using crontab, it will ask you to select an editor. If you are new to these editors, choose nano.

Set a cronjob to run every 5 minutes:
```
*/5 * * * * cronlock /usr/local/bin/wp-cli cron event run --due-now --path=/var/www/xxx/public_html > /dev/null 2>&1
```

Every minute:
```
* * * * * cronlock /usr/local/bin/wp-cli cron event run --due-now --path=/var/www/xxx/public_html > /dev/null 2>&1
```
> Note: Replace `xxx` with your actual user.

### Manual test:
Run this in your terminal:
```
/usr/local/bin/wp-cli cron event run --due-now --path=/var/www/xxx/public_html
```
The output of this command should be like this:
```
Executed the cron event 'action_scheduler_run_queue_rucss' in 0.026s.
Executed the cron event 'action_scheduler_run_queue' in 2.179s.
Executed the cron event 'rocket_saas_clean_rows_time_event' in 0.013s.
Executed the cron event 'rocket_saas_pending_jobs' in 0.008s.
Executed the cron event 'rocket_remove_saas_failed_jobs' in 0.007s.
Executed the cron event 'rocket_saas_on_submit_jobs' in 0.005s.
Success: Executed a total of 6 cron events.
```

### Multisite Cronjob
If you have a multi-site setup, it's not sufficient to run the cronjob once. It must run for each site:

```
#!/bin/bash
# Adjust path to WordPress root
WP_ROOT=/var/www/xxx/public_html

/usr/local/bin/wp-cli site list --field=url --path=${WP_ROOT} 2>/dev/null |     xargs -i -n1 /usr/local/bin/wp-cli cron event run --due-now --path=${WP_ROOT} --url="{}"
```
---

## Remove Really Simple SSL
SSL redirection and HTTPS handling are webserver tasks, not WordPress tasks.

Our TurboStack already handles this for you.

To deactivate and remove the plugin via WP-CLI:
```
wp plugin deactivate really-simple-ssl
wp plugin uninstall really-simple-ssl
```
---
## WP Rocket
We suggest WP Rocket for large static websites. It does not require extensive configuration and works great out of the box.

For slow static pages, this will also increase the performance.

### wp-rocket for nginx
For NGINX setups (non-varnish):  
Rocket-NGINX config: https://github.com/SatelliteWP/rocket-nginx

---
## WP Redis Installation
Using wp-redis is recommended for dynamic WordPress sites because it solves a performance problem that page caching alone cannot fix:

logged-in pages, carts, dashboards, and personalized content must be generated dynamically.

Install and activate the plugin via WP-CLI:
```
wp plugin install wp-redis
wp plugin activate wp-redis
```

## Configure Redis socket
If you have Redis installed on your server, installed via our TurboStack, you can configure it in wp-config.php:

```
$redis_server = array(
    'host'     => '/var/run/redis/redis.sock',
    'port'     => null,
    'database' => 1,
);
```
> Note: Make sure to assign different Redis DB IDs if multiple apps run on the same server.

Enable Redis via WP-CLI:
```
wp redis enable
Success: Enabled WP Redis by creating wp-content/object-cache.php symlink.
```

Delete all remaining transient data via WP-CLI:
```
wp transient delete-all
```
---

## Quick optimization: Index WP MySQL for Speed

Install it once, optimize and remove again:
```
wp plugin install index-wp-mysql-for-speed
wp plugin activate index-wp-mysql-for-speed
wp index-mysql enable wp_commentmeta wp_comments wp_options wp_postmeta wp_posts wp_termmeta wp_usermeta wp_users
wp plugin deactivate index-wp-mysql-for-speed
wp plugin uninstall index-wp-mysql-for-speed
```

If you see this error:
> Error: These tables are not found…

Use correct prefix:
```
wp index-mysql enable SsW6eC_commentmeta SsW6eC_comments SsW6eC_options SsW6eC_postmeta SsW6eC_posts SsW6eC_termmeta SsW6eC_usermeta SsW6eC_users
```

### Extra plugins
These are some extra plugins we recommend to use.

- User indexing: https://wordpress.org/plugins/index-wp-users-for-speed/
- Menu speedup: https://wordpress.org/plugins/speed-up-menu/

### Give wordpress more resources
For bigger environments, you can give wordpress more resources by adding the following to wp-config.php:
```
define('WP_MEMORY_LIMIT', '512M');
define('WP_MAX_MEMORY_LIMIT', '512M');
```
Note that if you increase the memory limit in WordPress, you should also increase the memory limit for PHP.

You can do this in the .user.ini file:
```
memory_limit = 512M
```
So WordPress cannot exceed PHP’s authority, it can only work _within_ it.

## Varnish Support
Activate Varnish addon in WP Rocket AND install the Hosted Power **mu-plugin**.

![Enable Varnish](img/enablevarnish.png)


> You can download the file [here](https://files.hosted-power.com/srv/hostedpower.zip)

Place `hostedpower.php` inside `/wp-content/mu-plugins`.

This helper plugin ensures the hosting environment, caching layers, and WordPress all communicate correctly for optimal performance.
It prevents cache-breaking behavior, enforces proper HTTPS detection, and ensures Varnish purges and full-page caching work reliably.

## Elementor Page Builder Speedup
Activate **Element Caching** if using Elementor.

This setting can be found in:
`Elementor > Settings > Features > Element Caching`

Activating Element Cache is beneficial, because Elementor dynamically assembles pages from many database queries, template parts, and metadata on every request.

By caching these repeated lookups in memory (Redis/Memcached), Elementor pages render significantly faster for logged-in users and dynamic views, reducing database load and improving overall performance.

---