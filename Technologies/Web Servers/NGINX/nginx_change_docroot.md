---
hidden: true
---
# Change your nginx docroot

Some applications may require you to use a different docroot than the typical directory of '~/public_html'. Other times, your own application deploy flow may require you to change NGINX' docroot, for example to point it to '~/current'. In this guide, we'll quickly go over some common situations and ways to set this up.

!!!
Note that this is mostly applicable for general applications. Many CMSes and frameworks have a custom docroot path, that we configure by default in nginx if you set up your application with the correct application type. Be sure to check your current settings before making any changes!
!!!

## 1. Using symlinks

The easiest way to make sure nginx reads application files in the correct directory, is simply by turning the default directory into a symlink, pointing to the desired directory. Consider the following default directory setup:

```
sander@web1:~$ ls -l
<...>
drwxr-xr-x  2 sander sander  4096 Apr 13 00:00 logs
drwxr-xr-x  2 sander sander  4096 Jan 13 09:24 nginx
lrwxrwxrwx  1 sander sander  4096 Apr 14 11:53 public_html
<...>
```

Let's say your app typically deploys with a directory 'releases/', which contains the actual different releases and a symlink 'current', linking to the most recent release. This is the default 'Deployer' setup. In that case, you can simply remove the existing, empty public_html directory and replace it with a symlink:

```
sander@web1:~$ rmdir public_html
sander@web1:~$ ln -s current public_html
sander@web1:~$ ls -l
<...>
lrwxrwxrwx  1 sander sander    11 Apr 14 11:53 current -> releases/56
drwxr-xr-x  2 sander sander  4096 Jan 13 09:24 nginx
lrwxrwxrwx  1 sander sander     7 Apr 14 11:53 public_html -> current
<...>
```


## 2. Using nginx docroot

Alternatively, you could change the actual path nginx looks for. In your home directory you'll find the NGINX config directory, containing the '50main.conf' file. Here, you'll find your main application NGINX config, including the docroot. Depending on your configured app type, the exact contents of this file will change. However, you can edit this file as desired. An example of how the docroot is defined in this file is below:

```
root /var/www/sander/public_html;
```

Here you could change the 'public_html' part to any path as required by your application.