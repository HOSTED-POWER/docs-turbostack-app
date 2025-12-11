---
hidden: true
---
# Redis integration
Here you'll find how to enable and integrate Redis for your specific CMS, on our TurboStack environment.

## General implementation check
After implementing Redis, you can verify if data is being cached by using the following commands:

```
redis-cli
127.0.0.1:6379> keys *
(empty array)
```
If **empty array** displays, it means that nothing has been cached yet.
Click around on your website, then try the commands again.

If it remains empty, check if the implementation is correct.

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
MedusaJS offers Redis integration, but this is not enabled by default. Enabling and configuring Redis, must be done via the _terminal_.

We recommend adding a TTL and a custom namespace per shop. This ensures that the cache is not shared between shops and reduces the chance of completely filling up Redis.
### Configuration
To start, we need to add the variables to the `.env` file in `~/<project>/<shopname>/.medusa/server/`:

```
# Enable the new caching system
MEDUSA_FF_CACHING=true

# Redis socket URL (percent-encoded)
CACHE_REDIS_URL=redis://%2Fvar%2Frun%2Fredis%2Fredis.sock

# Optional settings
CACHE_TTL=28800   # 8 hours in seconds
CACHE_PREFIX=shopname-cache:
```
> **Note:** The Redis URL for a Unix socket must be encoded like this (`%2F` instead of `/`).

Now we must enable the _feature flag_ and the Redis module in `~/<project>/<shopname>/.medusa/server/medusa-config.ts`. To do this, we need to incorporate the following code:

```javascript
import { defineConfig } from "@medusajs/framework/utils"

export default defineConfig({
  featureFlags: {
    caching: true,   // enable the feature flag
  },

  modules: [  //enable the caching module
    {
      resolve: "@medusajs/medusa/caching",
      options: {
        providers: [
          {
            id: "caching-redis",
            resolve: "@medusajs/caching-redis",
            is_default: true,
            options: {
              redisUrl: process.env.CACHE_REDIS_URL,
              ttl: process.env.CACHE_TTL
                ? parseInt(process.env.CACHE_TTL, 10)
                : undefined,
              prefix: process.env.CACHE_PREFIX,
            },
          },
        ],
      },
    },
  ],
})
```
### Make your requests cache

### Restarting PM2
To reload these new changes, restart the PM2 processes:

```
pm2 restart
```

