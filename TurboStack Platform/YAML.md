---
order: 100
icon: code-square
---
# YAML editor

Our platform offers a user-friendly web interface for server configuration. However, at its core, all server configurations are stored and managed using [YAML (YAML Ain't Markup Language)](https://yaml.org/). This editor provides you with direct access to your server's configuration in YAML format, granting you fine-grained control and flexibility.

Leveraging the power of Infrastructure as Code (IaC), this YAML representation allows for rapid server replication. By simply copying the YAML configuration and applying it to new servers, you can create fully identical setups in seconds. This ensures consistency and significantly reduces deployment time, making it ideal for scaling your infrastructure efficiently.

## What is YAML?

YAML is a human-readable data serialization language. Think of it as a way to represent complex data structures in a simple, text-based format. It uses indentation and simple syntax to define key-value pairs, lists, and nested structures. This makes it easy to understand and edit, even for users who are not professional developers.

## How can I edit my server's YAML?
You can easily switch from the GUI to the YAML view by pressing the following button:

![Yaml Source Code](../img/turbostackapp/YAML/source-code.png)

You could also choose to display YAML by default via the profile settings:

![Profile Settings](../img/turbostackapp/YAML/profile-settings.png)

## What can be done with the YAML editor?

- Direct Configuration: You can directly modify your server's settings, bypassing the standard web interface. This allows for advanced customization and troubleshooting.
- Advanced Settings: Access and configure parameters that may not be available through the point-and-click interface.
- Bulk Editing: Make changes to multiple server configurations efficiently by editing the YAML directly.
- Configuration Backup and Sharing: Easily copy and share your server configurations as YAML files.

!!!warning Important Considerations
- Power and Responsibility: The YAML editor is a powerful tool. However, it requires a good understanding of your server's configuration parameters. Incorrect modifications can lead to server instability or failure.
- No Built-in Validation: While the editor ensures valid YAML syntax, it does not validate the correctness of the configuration parameters themselves. You are responsible for ensuring your settings are valid.
- Refer to Documentation: Always consult the relevant documentation for your server's configuration parameters before making changes in the YAML editor.
- Backup Before Editing: Always create a backup of your existing YAML configuration before making any changes. This will allow you to revert to a working state if needed.
!!!

## What's the YAML structure

The server's configuration is organized into global settings and settings per system user and their associated web applications (vhosts).

### Global settings
The top-level of the YAML file contains global parameters that apply to the entire server. These settings define the overall environment and infrastructure. Some examples:

- `webserver`: the web server used (e.g., `nginx`).
- `mysql_version`: the version of the MySQL database server (e.g., `8.0`).
- `mysql_innodb_size`: the memory size of the MySQL database engine (e.g, `2G`)
- `postgresql_version`: the major version of the PostgreSQL database server (e.g., `17`).
- `elasticsearch_version`: the major version of ElasticSearch to install (e.g., `8.x`)
- `elasticsearch_heap_size`: the heap size of the ElasticSearch search engine (e.g., `2g`)
- `varnish_cache_size`: the size of the Varnish cache (e.g., `4g`)
- `redis_enabled`: whether the Redis cache is enabled, default: `true`.
- `redis_memory`: the memory to be used by the Redis cache (e.g., `5gb`).
- `firewall_country_block`: comma separated list of [ISO 3166 2-letter country codes](https://www.iso.org/obp/ui/#search) to block in the firewall (e.g., `CN,GB,RU,US`)
- `firewall_whitelist`: a list of public IPv4/IPv6 addresses and CIDRs to whitelist in the firewall.
- `ssh_keys`: a list of SSH public keys that will be allowed to login passwordless.
- `os_extra_packages`: a list of packages, to be installed by the package manager, required by the application.

A full list of global settings will come soon.

### system_users
The `system_users` section is a list of individual user configurations. Each user represents a Linux user with a dedicated home directory located at `/var/www/$username`. It can have these properties:
- `username`: The name of the system user (e.g., `prod`). This property is mandatory!
- `vhosts`: A list of virtual hosts (web applications) associated with the system user ([see below](#vhosts)).
- `ftp`: The ftp user configuration.

### vhosts
Each vhosts entry defines a web application hosted within the user's directory. The configuration can contain:
- `server_name`: The domain name(s) associated with the application. Multiple domains can be specified, separated by spaces (e.g., prod.domain.com prod.domain.eu).
- `app_type`: The type of application (e.g., `drupal`, `odoo`, `magento2`, `shopware`, `wordpress`, ...).
- `app_install`: A boolean value indicating whether the application should be automatically installed, or is managed by the customer.
- `php_version`: The PHP version used by the application (e.g., `8.4`, `8.3`, `8.2`, `8.1`, `8.0`, `7.4`, ...). This parameter is only relevant if the app_type uses php.
- `nodejs_version`: The nodejs major version used by the application (e.g. `22.x`).
- `cert_type`: The type of SSL certificate used (e.g., `letsencrypt`, `selfsigned` or `custom`).
- `docker_enabled`: A boolean value indicating whether the web app can use Docker.
- `rabbitmq_enabled`: A boolean value indicating whether the web app can use RabbitMQ.
- `varnish_enabled`: A boolean value indicating whether the web app can use Varnish.
- `k8s_enabled`: A boolean value indicating whether the web app can use K8S.

### Example
For clarity, consider this example with some global parameters and a `prod` and `dev` system user with some applications and a custom certificate.
```
---
webserver: nginx
mysql_version: 8.0

os_extra_packages:
- ffmpeg
- pwgen

elasticsearch_version: 7.x

firewall_country_block: CN,RU
firewall_whitelist:
- "1.2.3.4 # Home IP"
- "5.6.7.8/24 # Office range"

ssh_keys:
- ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDDKuK2+TIvERToLFqb+UYyJ/JZqia30Ksmd2Hsm/WML user@laptop
  
system_users:
- username: prod
  vhosts:
  - server_name: prod.mydomain.com prod.mydomain.eu
    app_type: my_app
    app_install: false
    php_version: 8.3
    nodejs_version: 22.x
    cert_type: letsencrypt
- username: dev
  vhosts:
  - server_name: dev.mydomain.com
    app_type: my_app
    app_install: false
    php_version: 8.4
    nodejs_version: 22.x
    cert_type: letsencrypt
  - server_name: megento2dev.mydomain.com
    app_type: magento2
    app_install: true
    cert_type: custom
    cert_pvk: |
      -----BEGIN PRIVATE KEY-----
      ...
      -----END PRIVATE KEY-----
    cert_fullchain: |-
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
  ftp:
  - user: dev-ftp
    homedir: /var/www/dev/ftp
```

### Templates
In the [templates section of the :rocket: TurboStack Platform](https://my.turbostack.app/templates), you'll find some more examples.
