---
order: 200
icon: terminal
---

# Turbostack CLI
The TurboStack Command Line Interface (later referred to as TSCLI) is available on all TurboStack nodes to provide you with an easy-to-use tool to manage the services on your environment, even ones you would normally need root access for. Below is a short description of the various features.

## TSCLI Commands
The TSCLI tool uses levels of arguments to categorize functions. Every command starts with **'tscli'** followed by the service you're managing, followed by the parameters for the function you're using as documented below.

### NGINX Webserver
[!badge icon="rocket" text="tscli nginx reload"] - Verifies the NGINX configuration and reloads it if valid. If it isn't valid you'll get an error with the issue reported.

[!badge icon="rocket" text="tscli nginx restart"] - Verifies the NGINX configuration and restarts it if valid. If it isn't valid you'll get an error with the issue reported.

### Apache Webserver
[!badge icon="rocket" text="tscli apache reload"] - Verifies the Apache configuration and reloads it if it valid. If it isn't valid you'll get an error with the issue reported.

[!badge icon="rocket" text="tscli apache restart"] - Verifies the Apache configuration and restarts it if valid. If it isn't valid you'll get an error with the issue reported.

### BlackFire php Profiler
[!badge icon="rocket" text="tscli blackfire enable"] - Installs the Blackfire Profiler and restarts the PHP-FPM service(s).

[!badge icon="rocket" text="tscli blackfire disable"] - Uninstalls the Blackfire Profiler and restarts the PHP-FPM service(s).

[!badge icon="rocket" text="tscli blackfire configure"] - Asks the user for Blackfire tokens.

[!badge icon="rocket" text="tscli blackfire reload"] - Restarts the Blackfire Profiler service, to apply changes to the configuration.

### Firewall
[!badge icon="rocket" text="tscli firewall check <ip>"] - Reports whether the IP is blocked, across both the Intrusion Prevention (IPS) layer (behavioral and manual blocks) and the firewall blocklist. Please make sure to only use valid IP addresses.

[!badge icon="rocket" text="tscli firewall block <ip>"] - Adds a firewall rule to block a specific IP address as specified in the IP parameter. Blocks permanently by default; use the `--time <seconds>` option for a temporary block that auto-expires (for example `--time 86400` for 24 hours). Use the `--comment` option to add a comment. 

[!badge icon="rocket" text="tscli firewall unblock <ip>"] - Removes the provided IP address from the firewall's deny list, and also clears any matching Intrusion Prevention (IPS) decision so it is not automatically re-blocked.

[!badge icon="rocket" text="tscli firewall display-blocks"] - Lists all active manual firewall blocks. Use `--full` to also get the automated blocks, we recommend piping the output to a file or a command-line text processing tool like grep or less.

[!badge icon="rocket" text="tscli firewall whitelist <ip>"] - Add the provided IP to both allow and ignore lists.

[!badge icon="rocket" text="tscli firewall unlist <ip>"] - Remove the provided IP from both allow and ignore lists.

[!badge icon="rocket" text="tscli firewall display-whitelists"] - Lists the active local firewall whitelist entries (the allow and ignore lists).

[!badge icon="rocket" text="tscli firewall flush"] - Flushes all automatic firewall IP blocks from the blocklist.

### PHP
[!badge icon="rocket" text="tscli php kill"] - Kills all the server's php-FPM processes.

### OPcache
[!badge icon="rocket" text="tscli opcache clear"] - Resets php's OpCache.


### MySQL
[!badge icon="rocket" text="tscli mysql restart"] - Restart MySQL. Use sparingly as this can add load to the server when restarting. Only works if MySQL is configured in turbostack.app

### PostgresQL
[!badge icon="rocket" text="tscli postgresql restart"] - Restart PostgresQL. Use sparingly as this can add load to the server when restarting. Only works if PostgresQL is configured in turbostack.app

### MongoDB
[!badge icon="rocket" text="tscli mongo restart"] - Restart MongoDB. Use sparingly as this can add load to the server when restarting. Only works if MongoDB is configured in turbostack.app

### Varnish Cache
[!badge icon="rocket" text="tscli varnish clear"] - Clears everything from Varnish Cache's memory.

[!badge icon="rocket" text="tscli varnish reload"] - Reloads the Varnish Cache configuration.

### Redis Cache
[!badge icon="rocket" text="tscli redis clear"] - Clears everything from Redis Cache's memory.

### Solr
[!badge icon="rocket" text="tscli solr restart"] - Restarts the Solr service.

### Docker

[!badge icon="rocket" text="tscli docker restart"] - Restarts the docker service. Only use this when docker cli is no longer working or sufficient.

### DKIM
[!badge icon="rocket" text="tscli dkim records"] - Show the required TXT records for DKIM.

[!badge icon="rocket" text="tscli dkim validate"] - Verify if the correct TXT records are active for DKIM to function correctly.

### RabbitMQ
[!badge icon="rocket" text="tscli rabbitmq queue list <vhost>"] - List the queues (and their sizes) for the given virtual host.

[!badge icon="rocket" text="tscli rabbitmq status"] - Show the message queue status and a live connectivity check.

### Service Status
[!badge icon="rocket" text="tscli service status"] - Shows a consolidated overview of all installed services on the node (web server, database, cache, message queue, search, container runtime, firewall and PHP-FPM) with their current status and whether they start on boot. This command does not require root access.

```
  SERVICE            UNIT               STATUS         ENABLED
──────────────────────────────────────────────────────────────
● Web server         nginx              active         enabled
● Cache              redis-server       active         enabled
● Cache              redis-persistent   active         enabled
● Database           mysql              active         enabled
● PHP-FPM            php8.3-fpm         active         enabled
  (... firewall, mail and other installed services follow)
```

Every service group above also supports its own **`status`** command for a focused view with per-instance liveness and uptime. A few examples:

[!badge icon="rocket" text="tscli nginx status"] - Web server status, including a configuration validity check.

[!badge icon="rocket" text="tscli redis status"] - Cache status with a live connectivity probe for each instance.

[!badge icon="rocket" text="tscli mysql status"] - Database status and liveness.

[!badge icon="rocket" text="tscli firewall status"] - Firewall status and protection mode.

```
Cache (redis)
  UNIT               ACTIVE             ENABLED    SINCE                          RESTARTS
● redis-server       active (running)   enabled    Mon 2026-05-18 07:37:09 CEST   0
● redis-persistent   active (running)   enabled    Fri 2026-03-13 16:16:14 CET    0
liveness:
  redis-server: ok (PONG)
  redis-persistent: ok (PONG)
```

### Application Background Services
Inspect the background services that run your application (queue workers, schedulers, and similar), across both supported process managers.

[!badge icon="rocket" text="tscli app status"] - Overview of your application's background services, with per-service liveness (active/total).

[!badge icon="rocket" text="tscli app list"] - A flat, scriptable list (user / backend / service / active-of-total), handy for piping into other tools.

[!badge icon="rocket" text="tscli app backends"] - Shows which process manager each application user uses.

```
▸ App services
  app1  uid 1002 · systemd
    ● main          systemd    1/1
    ● worker        systemd    1/1
    ● scheduler     systemd    0/1
```

### Log Search
[!badge icon="rocket" text="tscli logs <source> <query>"] - Searches your server logs using a plain-language query, entirely on the server.

The available sources are: **web** (nginx / apache), **ssh**, **database** (mysql / postgresql), **mail**, **cache** (redis), **queue** (rabbitmq), **firewall**, **system**, **php** and **cron**. For web logs, add `error` or `access` right after the source to scope the search; omit it to search both.

```
tscli logs nginx error from last hour
tscli logs database "show logs from yesterday"
tscli logs ssh "show last 50 log lines"
tscli logs web find timeout
```

Add `--explain` to see exactly how your query was interpreted (the matched source, time window, search terms and line limit). If you run a source without a query, the most recent log lines are shown.

```
$ tscli logs web find timeout --explain
▸ Interpreted  source=Web · action=search · terms=[timeout] · limit=30
```

### Health Check
[!badge icon="rocket" text="tscli healthcheck"] - Prints a quick server health snapshot. Add `brief` for a single-line summary.

```
$ tscli healthcheck brief
● Disk: 31% | Load: 0.30,0.28,0.27 (4) | Mem: 87%
```

## Tools

### Image Optimizer
[!badge icon="rocket" text="tscli tools image-optimizer <path>"] - Reduces the file size of JPG and PNG images found under `<path>`. By default it performs a **dry run** that only reports the potential savings; pass `--apply` to actually optimize, and originals are only replaced when the optimized version is smaller.

Options:
 - `-a, --apply` - Replace originals when the optimized file is smaller (without this, nothing is changed).
 - `-q, --quality <1-100>` - JPEG quality target used during compression (default `90`).
 - `-s, --size <pixels>` - Maximum width or height before the image is resized (default `2000`).
 - `-n, --new` - Only scan files that are new since the last run.
 - `--exclude <path>` - Skip a directory tree; may be passed more than once.

### Botload
We provide a *botload* tool that grants you more insight in your nginx / apache logs.
This is how you use it via our TurboStack CLI.

#### Features
 - List your logfiles and their size.
 - Show the amount of requests, and bot percentage.
 - Top 10 most common bots.
 - Top 10 IP addresses.
 - Hourly breakdown of requests.
 - Breakdown of requests (GET, POST, ...etc). 
 - 
#### Functions

[!badge icon="rocket" text="tscli tools botload list "] - Get a list of log files, select which one you want to analyze.

[!badge icon="rocket" text="tscli tools botload ip <IP address>"] - Search for a specific IP in all logs.

[!badge icon="rocket" text="tscli tools botload live"] - Live view of a specific log.

[!badge icon="rocket" text="tscli tools botload shared"] - Compact overview for shared packages.

[!badge icon="rocket" text="tscli tools botload list --time hh:mm:ss hh:mm:ss"] - Search between two given timeframes.

---
#### Example

The following, is an example of the main features. Take in mind that bigger logs, might take a couple of minutes. If the server has a high load, it will add on to the analysis time.

```
Detected Nginx – logs in /var/log/nginx

Available logs:

 1) access.log            0.3 MB
 2) access.log.1          0.2 MB
 3) error.log             0.0 MB
 4) error.log.1           0.0 MB
 5) prod.log             41.2 MB
 6) prod.log.1           34.3 MB
Select log (number or filename): 5

Analysis of prod.log:
Total requests:    141261
Crawler requests:  15880  (11.24%)

Top 10 bots:
     18636 bingbot
      5735 robot
      5443 ahrefsbot
       864 amazonbot
       263 semrushbot
       246 dotbot
       195 googlebot
        43 clarity-bot
        20 petalbot
        15 facebookexternalhit

Top 10 IPs:
      1478 2a03:2880:f800:13:: (crawler)
      1423 2a02:348:91:63c7::1
      1405 2a03:2880:f800:f:: (crawler)
      1387 2a03:2880:f800:3:: (crawler)
      1382 2a03:2880:f800:29:: (crawler)
      1379 2a03:2880:f800:36:: (crawler)
      1373 2a03:2880:f800:e:: (crawler)
      1373 2a03:2880:f800:44:: (crawler)
      1371 2a03:2880:f800:7:: (crawler)
      1360 2a03:2880:f800:1e:: (crawler)

Hourly breakdown:
  00:00  1573
  01:00  1823
  02:00  2577
  03:00  1408
  04:00  1133
  05:00  1552
  06:00  4698
  07:00  7296
  08:00  7951
  09:00  7522
  10:00  10518
  11:00  7106
  12:00  7544
  13:00  6948
  14:00  8651
  15:00  7419
  16:00  7348
  17:00  7146
  18:00  6872
  19:00  6987
  20:00  7718
  21:00  7203
  22:00  7240
  23:00  5028

Request methods:
    141189 GET
        53 HEAD
        18 POST
```
