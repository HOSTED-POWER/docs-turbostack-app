---
hidden: true
---
# Wordpress Hosting Reference Guide

We provide optimized server environments for running WordPress. As the hosting provider, we maintain the server and infrastructure, but WordPress configuration, themes, plugins, and customizations are your responsibility.

This guide outlines the file structure, recommended permission settings, and caching options for Wordpress.

## File Structure

Your WordPress installation resides in the following directories:

| Path                        | Purpose                                 |
| --------------------------- | --------------------------------------- |
| `~/public_html`             | WordPress root and document root.       |
| `~/public_html/wp-admin`    | Core WordPress admin interface files.   |
| `~/public_html/wp-content`  | Themes, plugins, and uploads directory. |
| `~/public_html/wp-includes` | Core WordPress functions and libraries. |
| `~/nginx`                   | Custom Nginx configurations.            |

## Permissions

WordPress requires correct file permissions to operate securely and reliably:

```bash
find ~/public_html -type f -exec chmod 644 {} \;
find ~/public_html -type d -exec chmod 755 {} \;
chown -R $USER:$USER ~/public_html
```

These settings ensure that files and directories have appropriate access without exposing security risks.

## Caching

Using caching solutions such as Redis and Varnish is highly recommended for WordPress. These tools help improve performance and reduce server load, especially for high-traffic websites.

For users who prefer managing cache through the WordPress dashboard, several popular plugins like **W3 Total Cache**, **WP Super Cache**, and **LiteSpeed Cache** provide user-friendly interfaces to control various caching layers (object, page, browser). These plugins can be configured to automatically purge cache upon content updates and offer integration with CDN services.

### Configuring Redis
You can find our documentation on how to set up Redis [here](../../../Miscellaneous/redis_integration.md)

### Clearing the cache
Advanced users with SSH access can also manually flush caching layers as needed:

```bash
tscli varnish clear
tscli redis clear
```

---

For questions about PHP versions, MySQL settings, or other hosting features, please contact our support team.
