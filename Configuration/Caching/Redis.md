---
order: 200
icon: cache
---

# Redis

Redis is a powerful in-memory database primarily used for caching and session management. Due to its speed and flexibility, Redis is widely used in high-performance environments such as web applications, e-commerce platforms, and real-time systems.

This page explains how our Redis setup works, why certain choices were made, and how to make the most of Redis.

---

## 1. Our Redis Setup: Two Instances for Optimal Performance

By default, at least two Redis instances run on our servers:

| Instance           | Port          | Purpose                   | Data Storage |
|--------------------|---------------|----------------------------|---------------|
| Redis              | 6379 (default) | Caching                   | Volatile      |
| Redis-persistent   | 6378 (custom)  | Sessions & critical data  | Persistent    |

### Why two Redis instances?

- **Faster cache processing:**  
  The standard Redis instance on port **6379** is used for **temporary cache storage**, improving **load times and reducing database strain**.

- **Reliable session storage:**  
  The second Redis instance on port **6378** is set for **persistent storage**, ensuring session data is not lost on restart. This instance should never be cleared, as it can influence the stability of the application.

> **Note:** While both instances save an RDB file to disk, only the `redis` instance is affected by the `tscli redis clear` command. This ensures persistence after a reboot and helps preserve critical session data even during maintenance or attack scenarios like DDoS.

---



### A. Use the Socket File for Local Performance

If Redis runs locally, we recommend using **socket files instead of network connections** to reduce overhead and latency.

- **Cache Redis socket file:**  
  `/var/run/redis/redis.sock`

- **Persistent Redis socket file:**  
  `/var/run/redis-persistent/redis.sock`

**Benefit:** Socket files improve communication speed and reduce CPU usage.

---

### B. Use Separate Databases for Multiple Websites/Applications

Redis supports **multiple databases (0-15)**. If running **multiple websites or applications** on the same server, use **separate databases**:

- Website A → `db 1`
- Website B → `db 2`

**Benefit:** Prevents applications from overwriting each other’s cache.  
**Note:** If more databases are needed, these can be configured.

---

### C. Set TTL and Expiry Properly

By default, Redis **does not** assign a TTL (Time-To-Live) to keys unless explicitly set.

- **Caching keys:** Set a TTL for **temporary data** (e.g., API responses, HTML fragments):
  ```bash
  SET key value EX 3600  # Expires after 1 hour
  ```

- **Sessions:** Use a longer TTL, such as **24 hours** for user sessions.

- **Critical data:** Do **not** set TTL for **important data** that must persist.

**Benefit:** Optimizes memory usage and prevents Redis from filling up completely.

---
## 2. Setting a TTL
Keeping your redis memory usage below the limit is very important, as a lack of free memory can cause several front-end issues, based on implementation within the application. To help you get this under control, we have some procedures for the more popular CMSes listed below.

### WordPress:

#### Cache plugin

Wordpress uses a redis plugin. The config file can be found here:

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

To monitor the change, you can use the analysis tool in [Redis Insight](https://docs.hosted-power.com/books/redis/page/redis-insight) and sort on TTL.

**Tip:** you can clear the Redis cache with the ' tscli redis clear ' command.

#### WP cache set TTL

#### Set default value

In wordpress one of the more common ways to add something to the cache is by using the 'wp_cache_set' instruction.

```
wp_cache_set( 'my_key', 'my_value', 'default', 600 );
```

#### Set value for specific namespace

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
const CACHE_LIFETIME   = 0;
```

This is again, an infinite TTL. Change it to a more reasonable number.

_This needs to be set for all the different namespaces._

### Drupal:

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

#### Set TTL per namespace

You can also set this per namespace in the settings:

```
nano public_html/sites/default/settings.php
```

Add the following:

```
$settings['redis.settings']['perm_ttl_cache_menu'] = 21600; // 6 hours
```

Now the 'menu' cache items will have a TTL of 6 hours.

All that remains is clearing the Drupal cache and flushing Redis:

```
drush cr
tscli redis clear
```
