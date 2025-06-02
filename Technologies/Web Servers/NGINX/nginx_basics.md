---
hidden: true
---
# NGINX on TurboStack

As our default (and recommended) webserver for TurboStack, NGINX has been extensively tweaked for optimal performance. As such, many "quick code snippets" you can find online can't just be pasted into the config file, as they often expect you to be modifying the base config.

## NGINX "includes" and your customizations

The NGINX folder is conveniently located in:

```
/var/www/<user>/nginx
```

This folder contains the NGINX configuration files for the application. A primary configuration file, named **50main.conf**, holds the general setup and directives. Additional custom configuration files can be placed in the same folder. For these custom files to be recognized by NGINX, they **must** end with **.conf**.

When Varnish is enabled, an extra directory **outside** is generated.

```
/var/www/user/nginx
├── 20rewrites.conf -> outside/main/20rewrites.conf (symlink)
├── 50main.conf
└── outside
    └── main
        ├── 20rewrites.conf
        └── 30headers.conf
```

In this case rewrite and header config files need to be places in the **~/nginx/outside** folder so Varnish picks it up. 

### Load order

The config files are read in alphabetical order. this means:

* **10cors.conf** is loaded before **50main.conf**.
* **50main.conf** will overwrite settings of **10cors.conf** in case of conflicting settings.  

This file inclusion system allows flexibility and maintainability: you can modularize related directives into separate, clearly named config files while keeping the core configuration in **50main.conf**.

### Config examples

#### Redirects (20rewrites.conf)

Non-WWW to WWW:

```
if ($host ~ ^(?!www\.)(?<domain>.+)$) {
    return  301 $scheme://www.$domain$request_uri;
}
```

Redirect to certain page:

```
if ($http_host ~* "^.*your-domain\.be$"){
    rewrite ^/$ https://your-domain/page/ redirect;
}
```

Redirect multiple domains to main-domain while keeping the URI:

```
if ($http_host ~* "^(.*)(first-site\.be|second-site\.nl|third-site\.eu)$"){
    rewrite ^(.*)$ https://www.main-site.com$request_uri redirect;
}
```

#### Security (50main.conf)

Allow access to robots.txt, disallow other **.txt** and **.log** files.

```
    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }
    
    location ~* \.(txt|log)$ {
        deny all;
    }
```

Disallow direct access to any php gile in a 'dot directory' like **.git/** .

```
location ~ \..*/.*\.php$ {
    return 403;
}

location ~ ^/sites/[^/]+/files/.*\.php$ {
    deny all;
}
```

This prevents execution of PHP files stored in hidden or unintended locations (like version control folders), enhancing security by blocking potential backdoors or sensitive scripts.

## Handy references

Official NGINX documentation:

```
https://nginx.org/en/docs/
```

Configuring CORS headers:

```
https://www.baeldung.com/linux/nginx-cross-origin-policy-headers
```