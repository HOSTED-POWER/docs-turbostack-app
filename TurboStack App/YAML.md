---
order: 170
icon: code-square
---

# YAML

_This page is under construction_

## How to add an account

### How to add a vHost to an account



```
---
webserver: nginx
redis_enabled: false
system_users:
  - username: custom
  - username: magento
    vhosts:
      - server_name: magentotest.com
        app_type: magento2
        php_version: "8.1"
        varnish_enabled: true
        rabbitmq_enabled: true
        cert_type: letsencrypt
  - username: shopware
  - username: akeneo
  - username: woocommerce
  - username: dotnet
  - username: django
  - username: odoo
    vhosts:
      - server_name: odootest.com
        app_type: odoo
        python_version: 3.11.8
        cert_type: letsencrypt
    ftp: []
  - username: Webshop1
  - username: Webshop2
  - username: Webshopx
```
