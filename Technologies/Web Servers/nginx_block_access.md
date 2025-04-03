---
hidden: true
---
# Use nginx to block access to your application

Sometimes, during development or under specific circumstances, you might want to block all unauthorized access to your web page or application. nginx allows you to do this quite easily and it even provides multiple ways to allow authorization:

#### The structure within the /var/www/`<user>`/nginx directory

The way our Turbostack works is it will create an nginx folder in the home directory of each system user with the following files:

```bash
magento@dylano-dev1:~/nginx$ ls -la

-rw-r--r-- 1 magento magento    0 Mar 14 09:20 20rewrites.conf
-rw-r--r-- 1 magento magento 3704 Feb  5 16:15 50main.conf
```

**50main.conf** will allow you to configure user-level webservice configuration for your domains. This file requires some knowledge of nginx, in case you are unsure of your change, please contact us to confirm your change.

**20rewrites.conf**  will allow you to configure redirects/rewrites from the domains on the server to a different (sub)domain or a specific page. (More on that in a different documentation page)

The structure of the nginx folder will be a bit different when **varnish is enabled**, then an outside folder will be created:

```
├── 20rewrites.conf
├── 50main.conf
├── mage.runmaps
└── outside
    └── main
        └── 20rewrites.conf
```

In the outside directory, you can define rewrites/redirects that need to bypass the varnish caching.

#### Enough about the structure, let's talk about protecting the environment.

##### 1\. Using IP-whitelisting 

Using deny all in the nginx configuration will block access to all IP addresses except those you explicitly allowed.
The best way to do this is by creating a file **10auth.conf** in the **/var/www/`<user>`/nginx** directory and place the whitelisting configuration in there, which will apply to the applications under that user. 

An example is shown below where all IP's are denied expect the IP's 178.238.102.146, 178.238.102.148 and 78.22.198.62.

```bash
magento@dylano-dev1:/var/www/magento/nginx# cat 10auth.conf
        allow 178.238.102.148; # openvpn
        allow 178.238.102.146; # Draytek VPN
        allow 78.22.198.62; # Dylano HP
        deny all;
```

If for example your Turbostack configuration of your server looks like this:

```yaml
---
webserver: nginx
mysql_version: "8.0"

elasticsearch_version: 7.x
system_users:
  - username: docker
    vhosts:
      - server_name: docker-learning.hosted-power.dev
        python_version: 3.10.10
        docker_enabled: true
        cert_type: letsencrypt
  - username: magento
    vhosts:
      - server_name: dylano-magento.hosted-power.dev dylano-magento2.hosted-power.dev
        app_type: magento2
        php_version: "8.3"
        cert_type: letsencrypt
```

Then this configuration will apply to the domains dylano-magento.hosted-power.dev and dylano-magento2.hosted-power.dev but not to docker-learning.hosted-power.dev as it belongs to a different user.

Another option would be to place it in the **50main.conf** file in the location you want to be protected by the whitelisting.
For example setting this in the **location /**  will protect the environment starting from the root location (e.g. www.test.com).
If you would set the whitelisting configuration in **location /page/** then [www.test.com/page](https://www.test.com/page) will be protected.

As the example provided below will protect **location /** :

```
    location / {
        try_files $uri $uri/ /index.php$is_args$args;
  
        allow 35.187.75.91;
        allow 87.233.217.242/28;
  
        satisfy any;
    }
```

!!!
When doing any changes within the nginx directory, you will have to reload the service to push the change:
tscli nginx reload
!!! 

##### 2. Enable Basic Authentication on your website for NGINX

It's relatively easy to configure Basic Authentication using a .htpasswd file (similar to a basic auth block in Apache .htaccess) in NGINX on TurboStack. This way you can block access to your development version of the website for non-authenticated users. This guide assumes you know what Basic Authentication is.

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
