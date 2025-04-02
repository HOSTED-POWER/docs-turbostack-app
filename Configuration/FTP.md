---
order: 210
icon: upload
---
# FTP

## What could cause a “Login failed” error ?

1. **Incorrect login details used**:
Login details used by users for FTP access include their username and password. If these credentials are given wrongly in the FTP client, it can result in a 530 login error in FTP.
   "530 Login authentication failed" errors also happen due to wrong password. Even a single additional space at the beginning or the back of a password can cause a login failure. 
2. **Path doesn't exist**:
When the FTP users is created, a path will be assigned on creation. If the path doesn't exist on the server, this will cause a login failure.
3. **No TLS used**:
TLS is required and enforced for security reasons. It's a very bad idea to disable it, since passwords would be sent in plaintext. Make sure to activate "Explicit SSL/TLS" in your FTP client. However, if you need to disable SSL anyway (For example incompatible ERP system that only supports plain text FTP)![Disable SSL resquirement for a single user](image/FTP/1715871329616.png "Disable SSL resquirement for a single user"), you can do so here:
