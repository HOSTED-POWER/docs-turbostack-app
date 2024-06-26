---
order: 150
icon: people
---
# Groups

Groups make it possible to alter settings across multiple nodes. e.g. time zones or SSH key access to one or multiple TurboStack nodes.

## Create a group

Open the Groups tab to efficiently manage settings without the need to make changes individually for each user or project. Create partner groups, which enables you to provide SSH key access to multiple employees across different end user or partner environments.

### Scenario 1: Use Groups for centralised user rights management with SSH keys

![SSHTab](../img/turbostackapp/groups/SSHTab.png)

Centralized user rights can streamline access control and simplify administration.

* Identify the different levels of access required within your system.
* Create user groups based on these access levels. For example, you might have groups like "admin", "developers", "sysadmins", etc.
* Add users to appropriate groups based on their roles and responsibilities.
* Each user who needs access to the system should generate an SSH key pair if they haven't already.
* Have users from different groups attempt to log in using SSH keys to ensure that access is restricted according to the group-based configuration.
* Regularly review user access and group memberships to ensure that they align with your current organizational requirements.

Hosted Power monitors system logs for any unauthorized access attempts and take appropriate action if necessary.

### Scenario 2: Use Groups for centralised TurboStack Node management

![HostsTab](../img/turbostackapp/groups/HostsTab.png)

Managing TurboStack nodes using groups can help streamline operations and ensure consistency across your infrastructure.

* Determine the different roles that your TurboStack nodes will fulfill, such as web node, database node, application node, etc.
* Create groups based on the identified server roles.
* Assign each TurboStack node to the appropriate group based on its role.

## The Security Tab

<div class="warning" style='background-color:#E9D8FD; color: #69337A; border-left: solid #805AD5 4px; border-radius: 4px; padding:0.7em;'>
<span>
<p style='margin-top:1em; text-align:center'>
<b>This is an advanced section, use with caution</b></p>
</span>
</div><br>

The **Security** tab can update the firewall for all nodes in the Group. Allowing (whitelisting of) specific IPs or IP ranges.`<br>`
You even have the ability to allow or block complete countries/regions.

![TurboStackAppSecurityTab](../img/turbostackapp/basicinstall/tsa_security_tab1.png)

1. Allow (whitelist) one or more IPs or IP ranges
2. Block (blacklist) one or more countries
3. Only allow (whitelist) one or more countries
