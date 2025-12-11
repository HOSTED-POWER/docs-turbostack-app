---
hidden: true
---
# Redis integration
Here you'll find how to enable and integrate Redis for your specific CMS, on our TurboStack environment.

## Drupal
To enable the redis module in Drupal, go to **Manage > Extend > Performance > Redis**  and enable it:
![Redis Cache Drupal](image/redis%20implementation/redis_drupal_enabled.png)

You can now verify that the module is acrually connected to the local Redis instance by clicking the **Configure** button. If it shows _Memory_ being used, it's configured correctly.

If you don't see _Memory_ being used, you might still need to configure Drupal to actually use Redis:

In the following file:

```
nano public_html/sites/default/settings.php
```

Add the following:

```
$settings['redis.connection']['interface'] = 'PhpRedis';
$settings['redis.connection']['host'] = '/var/run/redis/redis.sock';
$settings['redis.connection']['port'] = 6379;
$settings['redis.connection']['password'] = '';
$settings['redis.connection']['prefix'] = 'ProjectName:';
```
> **Note:** Don't forget to replace 'ProjectName' with your actual project name. and clear all caches after this change.
### Setting the default TTL
In the following file:

```
nano public_html/modules/contrib/redis/src/Cache/CacheBase.php
```

The following constant and function can be found:

```javascript
const LIFETIME_PERM_DEFAULT = 31536000;
...
if (($settings = Settings::get('redis.settings', [])) && isset($settings['perm_ttl_' . $this->bin])) {
  $ttl = $settings['perm_ttl_' . $this->bin];
}
else {
  // fallback to 31536000 = 1 year
}
```

This means: if *perm_ttl_cache_menu* is not set, fall back to a TTL of 1 year. This is the general setting for all Redis keys.  
If Redis Insight shows you really use only a couple of namespaces that can have a similar TTL, you can just edit this constant variable to the desired value.

### Setting the TTL per namespace
You can also set this per namespace in the settings:

```
nano public_html/sites/default/settings.php
```

Add the following:

```
$settings['redis.settings']['perm_ttl_cache_menu'] = 21600; // 6 hours
```

Now the 'menu' cache items will have a TTL of 6 hours.

### Clearing the old keys in the cache
To purge the old keys, you can use the following commands:

```
drush cr
tscli redis clear
```

## Wordpress
To enable the redis plugin, go to the _plugins_ page and search for _Redis Object Cache_, click on _settings_. Here you can enable the plugin by clicking **Enable Object Cache**.

![Redis Object Cache WordPress](image/redis%20implementation/redis_wp_enabled.png)

### Setting a TTL in object-cache.php
It's important to set a TTL for your cache keys. This will prevent your cache from filling up and causing issues with your website. Currently the plugin does not have the functionality to set a TTL via the interface. You can set it in the config file:

```
/var/www/XXXX/public_html/wp-content/object-cache.php
```

In this file we can find the default TTL that's set for Redis entries when none was given upon creation of the key:

```js
if ( ! defined( 'WP_REDIS_DEFAULT_EXPIRE_SECONDS' ) ) {
	define( 'WP_REDIS_DEFAULT_EXPIRE_SECONDS', 0 );
}
```

A TTL of 0 in Redis means 'Never Expire'. Change this value to for example 28800 seconds (8 hours).

To monitor the change, you can use the analysis tool in [Redis Insight](https://docs.turbostack.app/miscellaneous/redis_insight/) and sort on TTL.

> **Tip:** you can clear the Redis cache with the ' tscli redis clear ' command.

### Setting a TTL outside of object-cache.php
In wordpress one of the more common ways to add something to the cache is by using the 'wp_cache_set' instruction:

```
wp_cache_set( 'my_key', 'my_value', 'default', 600 );
```

Some plugins set keys to never expire, like the 'posts' keys. To find where a key is added, use the following command:

```
grep -Ri "wp_cache_set(" | grep 'posts'
```

This will give results like these:

```
wp-content/plugins/woocommerce-product-search/lib/core/class-woocommerce-product-search-service.php:     $cached = wp_cache_set( $cache_key, $posts, self::GET_TERMS_POSTS_CACHE_GROUP, self::get_cache_lifetime() );
```

In this file, it refers to itself ( **self::get_cache_lifetime()** ) to get the value. The function refers to the constant variable:

```
const CACHE_LIFETIME = 0;
```

This is again, an infinite TTL. Change it to a more reasonable number.

> **Note:** This needs to be set for all the different namespaces.

## Magento2

## Odoo

## Shopware

## MedusaJS
