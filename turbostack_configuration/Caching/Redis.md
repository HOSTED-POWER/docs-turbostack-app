---
order: 200
icon: database
---

# Redis

By default there are always at least 2 Redis instances available:

* redis
  * port 6379 (default)
  * socket file: /var/run/redis/redis.sock
  * Suitable for cache
  * Volatile data/information
  * Can be cleared at all times, shouldn't influence application when cleared
  * Data = non persistent (volatile)
* redis-persistent
  * port 6378 (non default)
  * socket file: /var/run/redis-persistent/redis.sock
  * Suitable for sessions
  * Persistent data/information
  * Should/will never be cleared, clearing would influence application
  * Data = persistent (non-volatile)


Notes:

* Using the socket file is recommended when using local redis instances (less overhead = better performance!)
* Try to use seperate databases when having multiple websites/applications on the same server (e.g. db 1 , db 2, ...)
* By default there are 16 databases available (0-15), this could be increased on request 
