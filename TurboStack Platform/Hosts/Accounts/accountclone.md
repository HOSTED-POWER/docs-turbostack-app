---
order: 500
icon: upload
---

#  Cloning an account

To make a clone of an existing application, we provide the **Clone Account** feature. This function replicates the entire configuration and copies the files and database, enabling a quick and efficient setup with minimal effort.

!!!Info
This feature is extremely useful to quickly copy your production environment to a staging environment or vice versa!
!!!

Here’s how to do it:

1. Navigate to the **Accounts** tab in the TurboStack Platform under your host.
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user6.png)
2. Click **Clone Account**.
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user7.png)
3. In the next step, select the source host and the account you want to clone. This can be either the current server or another server you manage.
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user8.png)
4. Choose the destination account, which can be either an existing or a new account, and choose whether to clone the database and/or files. Click **Next**. 
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user9.png)
5. Select the hostname(s) you'd like to associate with the account and choose the type of certificate you want to activate. Click **Next** to finalize.
![TurboStackNewUser](../../../img/turbostackapp/newapp/tsa_user10.png)

And that’s it! Your application is now cloned to a new account. Be sure to save the configuration and publish it when you're ready.