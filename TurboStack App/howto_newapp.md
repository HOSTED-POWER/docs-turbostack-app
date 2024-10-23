---
order: 170
icon: cloud
---
# Application Management

## How to create a new application

* Open the TurboStack app
* Click the host view
* Select to host to update

### Prerequisite

Creating a new user on the [TurboStack App](https://my.turbostack.app "TurboStack App").
An account must exist before an application can be configured.
How to create a new [account](./howto_newuser.md)

### Creating a new application in the GUI

Creating a new (default) application under the newly created `prod` user.
Scenario: creating a Magento2 application, listening on `www.example.com` and using varnish as caching

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app1.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app2.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app3.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app4.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app5.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app6.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app7.png)

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app8.png)

1. Open the detail section for the user
2. Click to add a new application
3. The first application for each user should always be `default`
4. Go to `Hostnames` and 1 or more names the website should listen on
5. Choose a website SSL certificate, there are 3 options: `letsencrypt`(default), `selfsigned` and `custom`(bring your own)
6. Go to `Technologies` and set the app type that matches your application
7. Enable PHP or another technology that your application requires
8. Scroll down to enable `varnish` on our websitedat
9. Optionally a `monitoring url` can be set that Hosted Power will monitoring 24/7.
10. Click `Save` to save and exit the configuration wizard.

Now, the new application is configured, click `Save & Publish` to deploy the configuration to the server.

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app9.png)
![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app10.png)

### Creating a new application in source code mode (YAML) [!badge icon="alert" text="Advanced"]

for more advanced users there also the YAML configuration.
adding a new application can be done with

```yaml
system_users:
  - username: prod
    vhosts:
      - server_name: example.com www.example.com
        app_type: magento2
        php_version: "8.2"
        varnish_enabled: true
        cert_type: selfsigned
```

> **Warning:**
> A system user, e.g., `prod`, is required before an application can be deployed.

![TurboStackNewApp](../img/turbostackapp/newapp/tsa_app11.png)
