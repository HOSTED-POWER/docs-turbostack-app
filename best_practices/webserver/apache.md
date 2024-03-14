---
order: 100
---

# Apache

==- **<span style="color:#c71a42">Grant or deny access to server for specific IP's while using basic auth</span>**

In this article, we'll tackle the problem how we can decide if a visitor should or should not login on a server with basic auht enabled, based on it's IP-adress.
 
So what is the result we want to achieve? We want to implement an .htpasswd so visitors need to have a valid login, except when the request came from a whitelisted IP adress.
In that case, no login is asked and you'll be redirected to the site. Like a VIP that would skip a waiting queue for a club.

#### Method 2: Server with varnish enabled

For a server with varnish enabled, is a different approach needed. All requests that go through varnish will pass the header (X-Forwarded-For), but it may contain some tempered information about the visitors IP.
Because of this modification, the request for immediate access will be denied and the visitor will be asked to login. To make sure this won't happen, we'll add a variable for the header that contains the whitelisted IP-adress.
The code below will do the trick:

AuthType Basic AuthName "Restricted Content" AuthUserFile /var/www/user/apache2/.htpasswd # For best practice will we add the IP's to the required list require ip # Only a person with valid credentials will be redirected require valid-user # We create the variables for the header like so (Ip should be written between quotes): SetEnvIF X-Forwarded-For AllowIP # Include the env variable Require env AllowIP

```
location / {
   fastcgi_param HTTPS on; 
   try_files $uri $uri/ /index.php$is_args$args; 
   auth_basic "Administrator ^`^ys Area"; 
   auth_basic_user_file $MAGE_ROOT/.htpasswd; # Whitelist Ip-adress allow ; 
   satisfy any; 
}
```
===