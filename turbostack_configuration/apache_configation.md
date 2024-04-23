---
order: 60
icon: browser
---

# Apache

## Grant or deny access to server for specific IP's while using basic auth

In this article, we'll tackle the problem how we can decide if a visitor should or should not login on a server with basic auth enabled, based on it's IP-address.
 
So what is the result we want to achieve? We want to implement an .htpasswd so visitors need to have a valid login, except when the request came from a whitelisted IP adress.
In that case, no login is asked and you'll be redirected to the site. Like a VIP that would skip a waiting queue for a club.

This method is used for Apache2

### Method 1: Server without varnish enabled
There is a difference when a server has or doesn't have Varnish enabled.
For now we'll make it simple assuming there is no interruption with any service like Varnish. In that case we'll use the next setup:

_For best practice, we put this code at the top of our .htaccess file_ 

```
AuthType Basic
AuthName "Restricted Content"
AuthUserFile /var/www/user/apache2/.htpasswd

# Required IP's will be granted access without login require ip
# Only a person with valid credentials will be redirected require valid-user
```

### Method 2: Server with varnish enabled
For a server with varnish enabled, is a different approach needed. All requests that go through varnish will pass the header (X-Forwarded-For), but it may contain some tempered information about the visitors IP.
Because of this modification, the request for immediate access will be denied and the visitor will be asked to login. To make sure this won't happen, we'll add a variable for the header that contains the whitelisted IP-adress.


The code below will do the trick:

_For best practice will we add the IP's to the required list require ip_

```
AuthType Basic
AuthName "Restricted Content"
AuthUserFile /var/www/user/apache2/.htpasswd

# For best practice will we add the IP's to the required list
require ip 
# Only a person with valid credentials will be redirected
require valid-user
# We create the variables for the header like so (Ip should be written between quotes):
SetEnvIF X-Forwarded-For  AllowIP
# Include the env variable
Require env AllowIP
```

## Block infamous bytespider bot

Sometimes a server can go high in load due to the infamous bytespider bot. This one can be excluded by implementing this piece of code inside the .htaccess: 

```
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{HTTP_USER_AGENT} Bytespider [NC]
    RewriteRule .* - [F]
</IfModule>
```
