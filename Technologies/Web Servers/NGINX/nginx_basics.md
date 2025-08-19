---
hidden: true
---

# Nginx Configuration Basics

Our implementation of NGINX offers you the possibility to modify its configuration to your application-specific needs. To do so, you can place **custom NGINX configs** in a per-account directory under:  

```bash
/nginx
```

This approach allows you to extend or override server behavior safely without interfering with the main system configs.

---

## Load Order of Config Files

NGINX processes config files in **alphabetical order**. This is important when deciding how to name your files.  

By default, you’ll find two core configs in the directory:  

- **`20rewrites.conf`** → handles rewrite rules  
- **`50main.conf`** → the main configuration block  

Since files are loaded alphabetically, anything you add with a **higher prefix number** than `50` will be loaded *after* the default `50main.conf`. Similarly, using a lower prefix (e.g. `10...`) ensures your rules are applied earlier.

**Tip:** To control priority and keep your configuration well-organised, always choose your filename carefully.

---

## Placement in Relation to Varnish

In case Varnish is enabled on your server, it sits in front of NGINX on our TurboStack. Your config placement determines whether it runs **before** or **after** Varnish:

- **Inside `/nginx`**  
  → Files here are loaded **after Varnish**. This is ideal for app-level rewrites, caching rules, or security headers.  

- **Inside `/nginx/outside/main`**  
  → Files here are loaded **before Varnish**. Use this if you need to manipulate requests at the edge, before they hit the Varnish layer.  

---

## Special Supported File Types

Apart from standard `.conf` files, two special file formats are supported:

- **`.runmaps` files**  
  These are commonly used for **Mage runmaps** (Magento routing definitions).  

- **`.http` files**  
  Example: `backend.http`. These can define backend-specific rules or HTTP-specific configs.  

This flexibility allows more modular configuration management, especially in multi-app setups.

---

## Reloading NGINX

After making changes, reload NGINX to apply them using:

```bash
tscli nginx reload
```

This ensures your new configuration is active without requiring a full restart.

**Tip:** If you get an error, a mistake has found its way into the config! The message should point you in the right direction to troubleshoot the issue. 

---

## Use Case Examples

### Rewrites (`20rewrites.conf`)

1. **Custom Rewrite Rule (after Varnish)**  
   Place in `/nginx/20rewrites.conf`:  

   ```bash
   location /blog {
       rewrite ^/blog$ /blog/ permanent;
   }
   ```

2. **Non-WWW to WWW Redirect**  
   Place in `/nginx/20rewrites.conf`:  

   ```bash
   if ($host ~ ^(?!www\.)(?<domain>.+)$) {
       return 301 $scheme://www.$domain$request_uri;
   }
   ```

3. **Redirect to a Specific Page**  
   Place in `/nginx/20rewrites.conf`:  

   ```bash
   if ($http_host ~* "^.*your-domain\.be$") {
       rewrite ^/$ https://your-domain/page/ redirect;
   }
   ```

4. **Redirect Multiple Domains to Main Domain (keep URI)**  
   Place in `/nginx/20rewrites.conf`:  

   ```bash
   if ($http_host ~* "^(.*)(first-site\.be|second-site\.nl|third-site\.eu)$") {
       rewrite ^(.*)$ https://www.main-site.com$request_uri redirect;
   }
   ```

---

### Pre-Varnish Whitelisting (`outside/main/10whitelist.conf`)

Place in `/nginx/outside/main/10whitelist.conf`:  

```bash
allow 192.168.0.0/24;
deny all;
```

---

### Magento Runmaps

Add a `.runmaps` file in `/nginx` for routing tweaks. This is mainly used for Magento routing configuration.

---

### Security (`50main.conf`)

Allow access to `robots.txt`, but disallow other `.txt` and `.log` files:  

```bash
location = /robots.txt {
    allow all;
    log_not_found off;
    access_log off;
}

location ~* \. (txt|log)$ {
    deny all;
}
```

## Official NGINX documentation:

```
https://nginx.org/en/docs/
```