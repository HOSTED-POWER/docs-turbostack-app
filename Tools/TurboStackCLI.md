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

### BlackFire php Profiler
[!badge icon="rocket" text="tscli blackfire enable"] - Installs the Blackfire Profiler and restarts the PHP-FPM service(s).

[!badge icon="rocket" text="tscli blackfire disable"] - Uninstalls the Blackfire Profiler and restarts the PHP-FPM service(s).

[!badge icon="rocket" text="tscli blackfire configure"] - Asks the user for Blackfire tokens.

[!badge icon="rocket" text="tscli blackfire reload"] - Restarts the Blackfire Profiler service, to apply changes to the configuration.

### Firewall
[!badge icon="rocket" text="tscli firewall check"] - Returns info on whether or not the IP parameter is listed in the iptables. Please make sure to only use valid IP addresses.

[!badge icon="rocket" text="tscli firewall flush"] - Flushes all automatic firewall IP blocks from the blocklist.

[!badge icon="rocket" text="tscli firewall block"] - Adds a firewall rule to block a specific IP address as specified in the IP parameter.

[!badge icon="rocket" text="tscli firewall unblock"] - Removes the provided IP address from the firewall's deny list.

[!badge icon="rocket" text="tscli firewall whitelist"] - Add the provided IP to both allow and ignore lists.

[!badge icon="rocket" text="tscli firewall unlist"] - Remove the provided IP from both allow and ignore lists.

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

### Docker

[!badge icon="rocket" text="tscli docker restart"] - Restarts the docker service. Only use this when docker cli is no longer working or sufficient.

### DKIM
[!badge icon="rocket" text="tscli dkim records"] - Show the required TXT records for DKIM.

[!badge icon="rocket" text="tscli dkim validate"] - Verify if the correct TXT records are active for DKIM to function correctly.

### RabbitMQ
[!badge icon="rocket" text="tscli rabbitmq queue list <OPTIONS> <VHOSTNAME>"] - List RabbitMQ queues.

### Tools
#### Botload
This tool allows you to analyze incoming traffic and get insights in the amount of bots connecting per website
[!badge icon="rocket" text="tscli tools botload list "] - Get a list of log files, select which one you want to analyze. Takes the optional `--time` parameter followed by two timestamps in "%H:%M:%S" format (e.g. `tscli tools botload list --time 07:15:00 08:15:00`)

[!badge icon="rocket" text="tscli tools botload shared"] - Perform the analysis on all available log files, showing the top 10 results. Takes the optional `--top-results`, `-n` parameter to show more/less results

[!badge icon="rocket" text="tscli tools botload ip <IP address>"] - Get the percentage of entries in all logs that come from the provided IP address. Takes the optional `--cidr` parameter to do the analysis for an IP range

[!badge icon="rocket" text="tscli tools botload live"] - Pick a log file to get a live analysis of all current incoming traffic. 