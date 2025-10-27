---
order: 1000
icon: info
---

# General

The **Hosts** section lets you configure and manage your TurboStack server. This section will serve as your reference guide to help you set up your application within minutes!

Before we get down to the details, let's look into some general functionality:

![General functions](img/tsa_server_header1.png)

#### 1. Switch between the GUI layout and the (advanced) YAML layout

You can deploy your server using the GUI, or if you're more of an infrastructure-as-code nerd, you can dive straight into the YAML. Any changes you make in either GUI or YAML will be reflected in the other, so feel free to try both!

For more information on configuring your server using YAML, click [here](yaml.md).

!!! info
If you want to set YAML as default, you can do so by clicking the icon on the top right and clicking 'Profile settings'. Here, you can switch between GUI and source.
!!!

#### 2. Revisions: shows all historic configuration changes made to the server

We all make mistakes, but we have you covered: The revision function allows you to review the full history of server configuration changes and revert to a past version at the click of a button.

#### 3. Fetch the credentials and IPs of the server's users and databases

The credentials button gives you a full overview of all your server credentials, including system users, database users, FTP users, etc.

#### 4. Save any changes made to the configuration

Save your changes when settings have been modified, without deploying the changes to the server.

#### 5. Save and Publish: saves and deploys the changes made to the server

Save the changes you've made to the configuration, and immediately deploy them to the server. There is also an option to save & full publish, which will ensure everything is deployed, as opposed to only the changes.

The following pages describe configuration options for all tabs in the host management console.