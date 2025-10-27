---
order: 1000
icon: plus-circle
---
# Setup

This article explains how to set up your application environment from scratch. If you want to replicate an existing environment, or want to migrate an application from an external server to TurboStack, please consult the [Account Cloning](accountclone.md) and [Migration Hero](migrationhero.md) articles respectively.

## What is an account and what is it used for?

The account is a linux user which can be used simply to access the server. However, you probably want to deploy one or multiple **applications** under this user. This guide will explain how to deploy system users and all settings your application requires.

!!!
For staging and production instances of an application, best practice is always to use an entirely different server, because it's by design unavoidable that resources allocated for staging, will be (silently) "taken away" from production. Even when this staging application is rarely used, it would still consume memory for e.g. databases, causing avoidable overhead.
!!!

## Creating a new account

### Deploying a new account in the GUI

Creating a new user on the [TurboStack Platform](https://my.turbostack.app "TurboStack Platform").

* Open the TurboStack Platform
* Open the server view

1. Go to the **Accounts** page
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user1.png)
2. Add a new account (user)
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user2.png)
3. Give the account a name and save
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user3.png)
4. **Save and Publish** will deploy the change to the host
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user4.png)

### Deploying a new account in the source YAML [!badge icon="alert" text="Advanced"]

Advanced users can also deploy an account in the source YAML:

```yaml
system_users:
  - username: prod
```

![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user5.png)

More info on using the YAML editor can be found [here](../yaml.md).

## Creating a new application

### Creating a new application in the TurboStack Platform GUI

Creating a new (default) application under the newly created **prod** user.
Scenario: creating a Magento2 application, listening on `www.example.com` and using varnish as caching

1. Open the detail section for the user
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app1.png)
2. Click to add a new application
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app2.png)
3. The first application for each user should always be **default**
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app3.png)
4. Go to **Hostnames** and 1 or more names the website should listen on
5. Choose a website SSL certificate, there are 3 options: **letsencrypt**(default), **self-signed** and **custom** (3rd party certificate) (*)
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app4.png)
6. Go to **Technologies** and set the app type (**) that matches your application
7. Enable PHP or another technology that your application requires
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app5.png)
8. Scroll down to enable **varnish**
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app6.png)
9. When going live, set a **monitoring url** so Hosted Power will monitor 24/7.
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app7.png)
10. Click **Save** to save and exit the configuration wizard.
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app8.png)

Now the new application is configured, click **Save & Publish** to deploy the configuration to the server.

![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app9.png)
![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app10.png)

(*) Let's Encrypt certificates can only be validated if the selected hostnames already have the proper DNS settings to point them to the server. If this condition is not met, the validation will fail, and publishing will throw an error! If you cannot adjust DNS, but you do need HTTPS, you can choose a self-signed certificate and change it to Let's Encrypt whenever you're ready.

(**) The app type should match the CMS or framework that will be installed on this environment, and automatically applies some application-specific configuration. Can't find your CMS of framework in this list? Contact us!

### Creating a new application in source code mode (YAML) [!badge icon="alert" text="Advanced"]

Advanced users can also deploy an application in the source YAML:

```yaml
system_users:
  - username: prod
    vhosts:
      - server_name: example.com www.example.com
        app_type: magento2
        php_version: "8.2"
        varnish_enabled: true
        cert_type: self-signed
```

!!! info
A system_user (e.g. **prod**) is needed before an application can be deployed
!!!

![TurboStackNewApp](../../../img/turbostackapp/newapp/tsa_app11.png)

More info on using the YAML editor can be found [here](../yaml.md).


