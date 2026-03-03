---
order: 210
icon: upload
---
# (S)FTP
## Configuration
We provide the possibility to make a (S)FTP user and choose the *home directory* for that user. This is mostly used to connect an ERP system to a webshop. 

The home directory of the (S)FTP user should not be the same as the system user's home directory. Instead it should only have access to the files and directories needed for your application.

For example:
  - A directory containing report PDF files.
  - A directory containing product images.
  - A directory containing stock exports.

(S)FTP users are created within the context of a normal system user.
## Creating the FTP user
### In the GUI
To create the user in the GUI, go to **Accounts**, click the *settings* **cogwheel** , then **Add FTP user**.

![Accounts tab showing FTP users](image/FTP/ftp_accounts_tab.png "Accounts tab showing FTP users")

A new window will appear, where you can enter the name for the new FTP user and the *home directory*. If your application requires it, you can enable TLS in the same screen.

When done, click **Save** to exit the window, then **Save & Publish** to create the FTP user
### In the YAML
The following YAML will create a FTP user under the *stag* user.
Example:
```yaml
system_users:
- username: stag
  vhosts:
  - server_name: demo.hosted-power.com
    app_type: shopware
    php_version: "8.3"
    cert_type: letsencrypt
  ftp:
  - user: stagftp
    homedir: /var/www/stag/public_html/ftp
```
### Enabling SFTP
You must either choose FTP or SFTP for all users on your server. To enable SFTP for all of them, add the following line above **system users**:
```
ftp_sftp: true
```
### Choosing the SFTP port
By default, the SFTP port is **222**. Some ERP systems require a specific port. In that case, you can add the following line above **system users**:
```
ftp_sftp_port: 2222
```
Change port 2222 to the port your ERP system requires.

---
## Connecting via (S)FTP
### Fetching the credentials
To connect to the FTP server, you need the username and password. These can be found by clicking the **Credentials** button in the top right corner of the TurboStack GUI.

![Credentials button](image/FTP/credentials.png)

This will show all available credentials for your TurboStack server.

### Connecting via FileZilla
Once you have the credentials of the (S)FTP user, you can connect to the FTP server using FileZilla, or another FTP client. 

Depending whether you use FTP or SFTP, you need to use a different port. FTP uses port **21**, while SFTP uses port **222**, unless you defined another port for SFTP.

---
## What could cause a “Login failed” error ?

1. **Incorrect login details used**:
Login details used by users for FTP access include their username and password. If these credentials are given wrongly in the FTP client, it can result in a 530 login error in FTP.
   "530 Login authentication failed" errors also happen due to wrong password. Even a single additional space at the beginning or the back of a password can cause a login failure. 
2. **Path doesn't exist**:
When the FTP users is created, a path will be assigned on creation. If the path doesn't exist on the server, this will cause a login failure.
3. **No TLS used**:
TLS is required and enforced for security reasons. It's a very bad idea to disable it, since passwords would be sent in plaintext. Make sure to activate "Explicit SSL/TLS" in your FTP client. However, if you need to disable SSL anyway (For example incompatible ERP system that only supports plain text FTP), you can do so here: ![Disable SSL resquirement for a single user](image/FTP/1715871329616.png "Disable SSL resquirement for a single user")

4. **IP is not whitelisted**:
Sometimes an IP can be (temporarily) blocked by the firewall. The best way to prevent this, is to whitelist the IP address that you use to connect to the FTP server. This can be done in the TurboStack GUI under:
```
Server > Security > Whitelist IP Addresses
```
![Whitelist IP Addresses](image/FTP/firewall_whitelist.png)
Whitelisting your mission critical IP adresses is a good practice.

---