---
order: 90
---

# DirectAdmin

## Use composer version 1 on DirectAdmin
Unfortunately DirectAdmin defaults to composer v2, despite efforts to override, this often get's updated to the newer version again.

The solution? Use "composer1 ..." command explicitly instead of "composer ..." to use the v1 version!

## Apache reverse proxy to docker, nodejs or any other service

It's possible to use reverse proxy on DirectAdmin quite easily, however it's important to not block for example Let's Encrypt.

We will use port **8086** as an example here, navigate as admin user to:
Server Manager >> Custom HTTPD Configurations >> "Select applicable domain" **httpd.conf** >> Customize >> Put the following code under **CUSTOM2**

```
RewriteEngine On
RewriteCond %{REQUEST_URI} !^\.well-known/(.*)
RewriteCond %{HTTPS} !=on
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
ProxyPreserveHost On

ProxyPass /.well-known !
ProxyPass "/" "http://localhost:8086/"
ProxyPassReverse "/" "http://localhost:8086/"
```
![Apache Reverse Proxy DirectAdmin](../img/turbostackapp/control_panels/directadmin-apache-reverse-proxy.png)


## Override e-mail in DirectAdmin

DirectAdmin sometimes has an unwanted default email from when using a web form. There are 2 possible solutions:

* Use an SMTP connector/plugin and configure it to use localhost
* Override the EMAIL tag in the php fpm setting
  1. Go to Custom HTTPD Configurations
  2. Open the "php-fpm.conf" from the affected website/account
  3. Inside php-fpm Global |CUSTOM1|
  4. Add the override, for example |?EMAIL=john.doe@example.com|
  5. Make sure to apply on all php versions
  6. Hit the save button
 
 ![Override mail DirectAdmin](../img/turbostackapp/control_panels/fix-email-from.png)

