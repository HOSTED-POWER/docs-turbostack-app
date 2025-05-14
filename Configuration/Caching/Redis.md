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

## 2. Best Practices for Using Redis

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

