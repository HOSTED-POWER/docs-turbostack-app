---
order: 70
---

# Nginx configuration

## Enable Basic Authentication on your website for NGINX

It's relatively easy to configure Basic Authentication using a .htpasswd file (similar to a basic auth block in Apache .htaccess) in NGINX on TurboStack. This way you can block access to your development version of the website for non-authenticated users. This guide assumes you know what Basic Authentication is.

## How-to
First locate your nginx main configuration file from the home directory of your user:

```
vim nginx/50main.conf
```
 
Then, uncomment the following lines to activate Basic Authentication:
```
location / {
#auth_basic "Administrator’s Area";
#auth_basic_user_file /var/www/staging/.secrets/htpasswd;
#satisfy any;
```

OPTIONAL: You can add whitelisting based on IP-addresses for connections that will not need to identify using the Basic Auth service, simply add "allow ":
```
location / {
auth_basic "Administrator’s Area";
auth_basic_user_file /var/www/staging/.secrets/htpasswd;
allow 35.187.75.91;
allow 34.76.59.175;
allow 34.76.201.228;
allow 87.233.217.242/28;
satisfy any;
```
 

Lastly, to enact your changes, reload the NGINX service:
```
tscli nginx reload
```
