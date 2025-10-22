---
order: 190
icon: cache
---

# Varnish

Varnish is an optional caching service available in your Turbostack control panel. When you activate this option, the service will be installed and a default configuration provided. You may still need to configure your application to make full use of Varnish. 

Varnish default port: 6081 

## Varnish configuration

Our Turbostack has a default VCL config file, based on  the `app_type` defined in Turbostack App. Using your system user(s), you can read these files in `/etc/varnish/conf.d`, the file called 50main.vcl is our default config, based on a default VCL for your CMS where appropriate, expanded with our own optimizations. The following config is our `default.vcl` file, which will be loaded in and will then load in the contents of the conf.d directory mentioned earlier:

```
vcl 4.1;
import std;

include "/etc/varnish/backend.vcl";
include "/etc/varnish/acl.vcl";

include +glob "/etc/varnish/conf.d/*.vcl";

sub vcl_recv {
    # Happens before we check if we have this in cache already.
    #
    # Typically you clean up the request here, removing cookies you don't need,
    # rewriting the request, etc.
}

sub vcl_backend_response {
    # Happens after we have read the response headers from the backend.
    #
    # Here you clean the response headers, removing silly Set-Cookie headers
    # and other mistakes your backend does.
}

sub vcl_deliver {
    # Happens when we have all the pieces we need, and are about to send the
    # response to the client.
    #
    # You can do accounting or modifying the final object here.
}
```

As defined here, all .vcl files in the `/etc/varnish/conf.d` directory will be loaded in when Varnish is reloaded. If you want to add extra/custom configurations, your users have the necessary permissions to add files in this directory. Files are loaded in alphabetical order, which we use by prefixing all files with an index to determine their loading order, with the lower indexes getting loaded in first.

For example, creating a new file called 20_custom.vcl when the current directory structure looks like this:

```
prod@sander:/etc/varnish/conf.d$ ls -la
total 28
drwxrwxrwx 2 root root  4096 Oct 20 13:46 .
drwxr-xr-x 3 root root  4096 Sep 11 15:41 ..
-rw-r--r-- 1 root root  1007 Sep 11 15:41 10_bypass.vcl
-rw-r--r-- 1 root root 14799 Oct 13 15:06 50_main.vcl
prod@sander:/etc/varnish/conf.d$
```

would make the config you add to the 20_custom.vcl file be loaded in after the config from the 10_bypass.vcl file, but before the directives in the 50main.vcl file.

Importantly, to reload Varnish after you make any changes, you can use [!ref  TSCLI](/Tools/turbostackcli.md):

`tscli varnish reload`

## Varnish inner workings

Varnish functions as a sort of reverse proxy caching service. The requests sent to your application are first passed through Varnish, and modified as defined in the VCL config, or served entirely from its own cache memory.

Varnish does this by storing images, full pages, etc. in RAM for fast access, removing the need for your application to process the request fully itself, saving your server resources and increasing performance.

When a request comes in that doesn't yet have a response in its cache, or when otherwise defined in the VCL, it is passed to the webserver again, this time to be handled by the actual application. Then, if possible, the response is stored by Varnish along with some metadata, most importantly the 'TTL' or 'Time To Live' which indicates how long Varnish should keep the information in RAM.

Due to this, it is important to check for optimizations specific to your application as well, to make sure unnecessary data isn't being kept in memory and never retrieved, or data isn't being kept with a TTL that is too long. These could cause increased memory usage and lead to degraded performance of your server (and notifications from us about your memory/swap usage)

## Varnish links

Here's some useful links to the Varnish docs, like the 'Tutorials':

https://varnish-cache.org/docs/7.7/tutorial/

Or for other versions:

https://varnish-cache.org/docs/index.html

Do note these tutorials include steps to setup Varnish yourself, but we already provide a Varnish instance installed and running.