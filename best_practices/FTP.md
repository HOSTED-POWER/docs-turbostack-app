---
order: 90
icon: upload
---

# FTP

## What causes the “Login failed” error ?

1. Incorrect login details used
Login details used by users for FTP access include their username and password. If these credentials are given wrongly in the FTP client, it can give a 530 login error in FTP.
But for additional FTP accounts, the FTP login name is of the format ‘username@domain.com’. If the FTP username entered is not in this specific format, login failures happen.
“530 Login authentication failed” also happens due to wrong password. Even a single additional space in the password can cause a login failure. Many account owners tend to overlook that aspect and struggle with 530 errors.

2. Path doesn't exist
When the FTP users gets created by the support team, a path will be assagined on creation. If the path doesn't exists on the server this will cause a Login failure.

3. Unsupported TLS 
Of course, we can use **TLSRequired off** in our ProFTPd configuration as this allows for TLS and non-TLS logins, but if you want to make your FTP setup as secure as possible, you should enforce the use of TLS and make exceptions only for the users or groups that use an FTP client that doesn't support TLS (if using another FTP client is not an option for those users).

If we want to allow the FTP user testuser to use plain FTP instead of FTP, we can configure this as follows:
```
PATH: /etc/proftpd/conf.d/99_override.conf 
TLSRequired off
```
