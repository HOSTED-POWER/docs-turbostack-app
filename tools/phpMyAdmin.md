---
order: 100
icon: columns
---
# phpMyAdmin

 Introduction

phpMyAdmin is a web-based tool designed to manage MySQL and MariaDB databases. It provides a user-friendly graphical interface, allowing users to perform database tasks without needing to write SQL commands manually. With phpMyAdmin, you can create, modify, and delete databases, tables, fields, and records, as well as execute SQL queries, manage user permissions, and import/export data.
On Turbostack servers with MySQL enabled, phpMyAdmin comes pre-installed by default, making database management convenient and accessible right out of the box!

## How to access phpMyAdmin

To access phpMyAdmin, we need the following:
* The credentials of your SSH and database users, found in the 'credentials' tab in the [TurboStack App](https://my.turbostack.app "TurboStack App").
* A functional web browser

### Via the Turbostack App

phpMyAdmin is easily accessible via the [TurboStack App](https://my.turbostack.app "TurboStack App"). To locate the link, go to `Hosts` > `Manage` > `Accounts` > `Gear icon (x2)` > `Database info`. From there, click the Go to phpMyAdmin link.
Once there, you will encounter a password prompt. Enter your SSH user credentials (account name and password) to proceed.
This will take you to the phpMyAdmin login screen, where you need to enter your database credentials. After logging in, you can manage your databases, including tasks like creating, editing, and deleting databases, tables, and records, running SQL queries, and managing user privileges.

### Via a web browser

If, for any reason, this link is inaccessible, you can still reach phpMyAdmin directly through your web browser. You can do so by opening your web browser and navigating to the default URL with the '/phpadm' URI. For example: https://www.yourdomain.ext/phpadm. From here, you can continue with the same method as described above.