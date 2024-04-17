---
order: 20
---

# TS CLI

The TurboStack Command Line Interface (later referred to as TSCLI) is available on all TurboStack servers to provide you with an easy to use tool to manage the services on your server, even ones you would normally need root access for. Below is a short description of the various features.

 
## TSCLI Commands
The TSCLI tool uses levels of arguments to categorize functions. Every command starts with 'tscli' followed by the service you're managing, followed by the parameters for the function you're using as documented below.
 
Example:
 
tscli nginx reload
This command will reload the NGINX service configuration

 

### NGINX Webserver
[!badge icon="rocket" text="tscli nginx reload"] - Verifies the NGINX configuration and reloads it if it is valid. If it isn't valid you'll get an error with the issue reported.

### BlackFire php Profiler
[!badge text="test"]
[!badge icon=:icon-rocket: text="tscli blackfire enable"] - Installs the Blackfire Profiler and restarts the PHP-FPM service(s).
[!badge icon=":icon-rocket:" text="tscli blackfire enable"] - Uninstalls the Blackfire Profiler and restarts the PHP-FPM service(s).
[!badge icon="rocket" text="tscli blackfire reload"] - Restarts the Blackfire Profiler service, to apply changes to the configuration.

### Firewall

tscli firewall check

Returns info on wether the IP parameter is listed in the iptables. Please make sure to only use valid IP addresses.
tscli firewall block

Adds a firewall rule to block a specific IP address as specified in the IP parameter.
tscli firewall unblock

Removes an IP address from the firewall's deny list.
php OpCache:

tscli opcache clear

Resets php's OpCache
Varnish Cache:

tscli varnish clear

Clears everything from Varnish Cache's memory.
tscli varnish reload

Reloads the Varnisch Cache configuration
