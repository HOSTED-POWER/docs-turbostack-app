---
order: 180
icon: person
---

# TS Accounts Mgmt

## Creating a new account (linux user)

### Linux user

The account is a linux user. Later an application can be deployed under this user.
We do not allow to run an application on the root user for security reasons.

Further more we also recommend not to run different types of applications under the same user.
for example, the staging and production version of the application should be deployed on a separate
user and maybe even on entirely different server


### How to deploy a new account in the GUI

Creating a new user on the <a href="https://my.turbostack.app" target="_blank">TurboStack App</a>.

* Open the TurboStack App
* Open the server view

![TurboStackNewUser](../img/turbostackapp/newapp/tsa_user1.png)

![TurboStackNewUser](../img/turbostackapp/newapp/tsa_user2.png)

![TurboStackNewUser](../img/turbostackapp/newapp/tsa_user3.png)

![TurboStackNewUser](../img/turbostackapp/newapp/tsa_user4.png)

1. Go to the accounts page
2. Add a new account (user)
3. Give the account a name and save
4. `Save and Publish` will deploy the change to the host


### How to deploy a new account in the YAML [!badge icon="alert" text="Advanced"]

for more advanced users there also the YAML configuration.
adding a new account can be done with

```yaml
system_users:
  - username: prod
```

![TurboStackNewUser](../img/turbostackapp/newapp/tsa_user5.png)



Now an account is created. Applications can be installed.

Example, [Deploy a Magento2 application](./howto_newapp.md)
