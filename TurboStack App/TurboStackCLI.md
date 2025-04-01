---
order: 140
icon: terminal
---

# TS CLI
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


### Varnish Cache
[!badge icon="rocket" text="tscli varnish clear"] - Clears everything from Varnish Cache's memory.

[!badge icon="rocket" text="tscli varnish reload"] - Reloads the Varnish Cache configuration.

### Redis Cache
[!badge icon="rocket" text="tscli redis clear"] - Clears everything from Redis Cache's memory.

### DKIM
[!badge icon="rocket" text="tscli dkim records"] - Show the required TXT records for DKIM.

[!badge icon="rocket" text="tscli dkim validate"] - Verify if the correct TXT records are active for DKIM to function correctly.

### RabbitMQ
[!badge icon="rocket" text="tscli rabbitmq queue list <OPTIONS> <VHOSTNAME>"] - List RabbitMQ queues.

