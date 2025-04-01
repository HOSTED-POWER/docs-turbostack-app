---
order: 100
icon: code-square
---
# YAML editor (Infrastructure-as-code)

_This page is under construction_

The server configuration source code is available in YAML format. This allows for the rapid creation of new, fully identical servers within seconds.

[!embed allowFullScreen="false"](https://player.vimeo.com/video/1053693836?title=0&amp;byline=0&amp;portrait=0&amp;badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479)

Detailed screenshots can be found below

![Yaml Source Code](../img/turbostackapp/YAML/source-code.png)

You could also choose to display YAML by default via the profile settings.

![Profile Settings](../img/turbostackapp/YAML/profile-settings.png)

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
