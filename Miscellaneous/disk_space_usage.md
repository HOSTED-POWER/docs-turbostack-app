---
hidden: true
---

# Manage your disk space usage

### There's a lot of risk in running out of disk space!

Not only will it cause downtime when services inevitably crash when no more space is available, this may cause data corruption that persists even after space is freed up, that will necessitate restoring from backup and thus lead to data loss. As such, we try to make sure you're always informed when your disk is filling up, but an ounce of prevention is worth a pound of cure so we recommend using the following tips and tricks to keep a watchful eye on your application's disk usage

### **How to check disk space usage**

Proactively checking the disk usage before pushing an update or a new release is a must if you want to avoid the chances of your server crashing. To start off with the first tool: 

#### df

This command-line tool is used to check disk usage on a mounted filesystem, displaying the total size of the disk space, the amount of disk space used, the amount available and on which path it is mounted.

In the example below, we see the option '-h' has been defined, which makes the output human-readable. ( Use df --help in case you want to dive deeper into the options the command provides)

```

ssh-user@dylano-dev1:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           388M  2.2M  386M   1% /run
/dev/vda2        30G   17G   12G  59% /
tmpfs           1.9G  1.2M  1.9G   1% /dev/shm
```

Here we see that the filesystem **/dev/vda2** has been mount on the root directory **/,** meaning that when you upload new images to for example /var/www/username/public\_html/media, it will use the filesystem /dev/vda2 as it belongs to a subdirectory of the root directory / where the disk is mounted.

**/dev/vda2** is the root disk of your server and will be the one that is defined in our turbostack plans.

The next command is the best one suited for your needs as a customer to dive deeper into how much disk space each directory takes up:  

#### **ncdu** (**NC**urses **D**isk **U**sage)

This is a powerful, interactive tool that provides a more detailed and user-friendly way to analyze disk usage. It offers a visual representation of disk usage, making it easier to identify large files and directories.

As an SSH user on the server, you will be able to scan everything within your home directory.

As an example, we will start scanning the **current** directory (which in this case will be the home directory **/var/www/prod/**)

```
prod@web2:~$ ncdu .
```

```
--- /var/www/prod -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  304.2 GiB [##########################] /magento2
    1.4 GiB [                          ] /.db.dumps
    1.2 GiB [                          ]  prod_db.sql.gz
  838.9 MiB [                          ] /.npm
  235.7 MiB [                          ] /.cache
  124.3 MiB [                          ] /indexer-stats
   76.0 KiB [                          ] /.pki
   64.0 KiB [                          ] /logo-found-which-was-not-in-git
   24.0 KiB [                          ] /.local
   20.0 KiB [                          ] /nginx
   12.0 KiB [                          ] /.config
   12.0 KiB [                          ] /.ansible_async
   12.0 KiB [                          ] /.ssh
   12.0 KiB [                          ] /.sshtmp
    8.0 KiB [                          ] /.gnupg
    8.0 KiB [                          ] /.ansible
```

You can navigate to a (sub)directory by pressing the enter button:

```
--- /var/www/prod/magento2 ----------------------------------------------------------------------------------------------------------------------------------------------------------------
                                         /..
  289.9 GiB [##########################] /shared
   14.1 GiB [#                         ] /releases
  139.3 MiB [                          ] /repo
```

By navigating deeper within these subdirectories, it will be easier to pinpoint which directory is consuming the most storage space:

```
--- /var/www/prod/magento2/shared/pub/media/catalog/product -------------------------------------------------------------------------------------------------------------------------------
                                         /..
  263.1 GiB [##########################] /cache
    4.6 GiB [                          ] /p
    2.3 GiB [                          ] /3
    1.6 GiB [                          ] /2
    1.0 GiB [                          ] /c
```

While `ncdu` is a powerful tool for analyzing disk usage, it also allows you to delete files and directories directly from its interface. This can be very useful, but it also comes with the risk of accidentally deleting important data.
To delete a file or directory in `ncdu` you would have the press the **d** key, wherafter a confirmation of deletion pop-up will be shown, pressing enter afterwards will **permanently** delete the concerning directory and/or files.

For more information about this tool, I'd like to refer you to [Ncdu 2.7 Manual](https://dev.yorhel.nl/ncdu/man)

### **Common causes**

#### 1\. Old Backups

Backups are essential for data recovery, but they can consume a significant amount of disk space if not managed properly. Over time, old backups that are no longer needed can accumulate and take up valuable storage. Regularly reviewing and deleting outdated backups can help free up space. 

Be aware we take backups of your entire environment daily with a retention period of 20 days on a separate server.  It could be a backup was taken before pushing a new release in case a backup plan was needed but taking general daily backups of your environments is redundant as backups are taken from our side as well and your backups will unnecessarily consume disk space.

#### 2\. Log Files

Log files are generated by various applications and services running on the server. These files can grow very large, especially if the server is handling a lot of traffic, debugging is enabled or if there are errors being logged frequently. It’s important to monitor log file sizes and implement log rotation policies to archive or delete old logs automatically. 

#### 3\. Caching Folders

Caching is used to improve performance by storing frequently accessed data. However, cache folders can grow significantly over time, especially if they are not cleared regularly. Applications like web browsers, package managers, and web servers often use caching, and their cache directories can become quite large.

#### 4\. Temporary Files

Temporary files are created by various processes and applications. While these files are meant to be temporary, they can sometimes be left behind and accumulate over time. Regularly cleaning up temporary directories can help reclaim disk space.

Please **confirm with your development team** before deleting files / directories when you are not sure about the status of these files.
