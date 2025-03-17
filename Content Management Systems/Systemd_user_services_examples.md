---
order: 100
icon: apps
---

# What are systemd user services?

You are probably familiar with the system-wide systemd services, managed by the systemctl command. However, systemd services can also be entirely controlled by regular unprivileged users.
So it means to configure this, you don't have the need for root access, you can run the services under a regular user account.

Here are some examples on how to configure and utilise the user-level systemd services for a variety of content Management systems

### Akeneo

Create a file:

`~/.config/systemd/user/pim_job_queue@.service`

```
    [Unit]  
    Description=Akeneo PIM Job Queue Service (#%i)  
    After=network-online.target  
    Requires=dbus.socket  
    StartLimitIntervalSec=0  
    
    [Service]  
    Type=simple  
    WorkingDirectory=%h/akeneo  
    ExecStart=%h/akeneo/bin/console messenger:consume --env=prod %I  
    RestartSec=10s  
    Restart=always  
    
    [Install]  
    WantedBy=default.target  
```
**Notes:**

*   Keep the `Requires=dbus.socket` and `After=network-online.target` parts, it is important for the user services to use the proper dependencies.
*   `StartLimitIntervalSec=0` allows endless retrying to restart the service every 10s. (Otherwise it will stop trying when it fails after a while)
*   Also keep the `WantedBy=default.target` because not all other targets are available in systemd user services.
*   You cannot depend on the system services like mysqld, it will not be found by the user services.

**Now enable and start some instances, for example:**

```
    # 2 Instances of ui_job
    systemctl --user enable pim_job_queue@ui_job{1..2}
    systemctl --user start pim_job_queue@ui_job{1..2}
```

```
    # 1 Instance of import_export_job
    systemctl --user enable pim_job_queue@import_export_job{1..1}
    systemctl --user start pim_job_queue@import_export_job{1..1}
```

```
    # 1 Instance of data_maintenance_job
    systemctl --user enable pim_job_queue@data_maintenance_job{1..1}
    systemctl --user start pim_job_queue@data_maintenance_job{1..1}
```

Restart it:

`systemctl --user restart pim_job_queue@*.service`

Status:

`systemctl --user status pim_job_queue@*.service`

### Laravel

`~/.config/systemd/user/laravel-queue@.service`
```
    [Unit]  
    Description=Laravel queue worker (#%i)  
    After=network-online.target  
    Requires=dbus.socket  
    StartLimitIntervalSec=0  
    [Service]  
    Type=simple  
    WorkingDirectory=%h/app  
    ExecStart=/usr/local/php74/bin/php artisan queue:work --daemon --env=prod  
    RestartSec=10s  
    Restart=always  
    [Install]  
    WantedBy=default.target
```
### CraftCMS

~/.config/systemd/user/craftcms-queue@.service
```
    [Unit]  
    Description=CraftCMS queue runner (#%i)  
    After=network-online.target  
    Requires=dbus.socket  
    StartLimitIntervalSec=0  
    [Service]  
    Type=simple  
    WorkingDirectory=%h/domains/example.com/craftcms  
    ExecStart=/usr/local/php80/bin/php %h/domains/example.com/craftcms/craft queue/listen  
    ExecReload=/bin/kill -SIGABRT $MAINPID  
    RestartSec=10s  
    Restart=always  
    [Install]  
    WantedBy=default.target
```

After creating this systemd file, make sure to disable the `runqueueautomatically` in the general configuration settings!

See: [https://craftcms.com/docs/4.x/config/general.html#runqueueautomatically](https://craftcms.com/docs/4.x/config/general.html#runqueueautomatically "runqueueautomatically")

### Shopware

**Information from shopware**

>Shopware uses the Symfony Messenger component and Enqueue to handle asynchronous messages, allowing tasks to be processed in the background. Consequently, tasks can be managed independently of timeouts or system crashes.

>By default, tasks in Shopware are stored in the database and processed via the browser, provided you are logged into the administration.
This method is simple and fast for the development process but is not recommended for production systems. With multiple users logged into the administration, this can lead to high CPU load and interfere with the smooth execution of PHP-FPM.

Create a file:

`~/.config/systemd/user/shopware_consumer@.service`

```
    [Unit]
    Description=Shopware Message Queue Consumer (#%i)
    After=network-online.target
    Requires=dbus.socket  
    StartLimitIntervalSec=0

    [Service]
    Type=simple
    WorkingDirectory=%h/shopware
    ExecStart=/usr/bin/php8.1 %h/shopware/bin/console messenger:consume --time-limit=3600 --memory-limit=2G --limit=25
    RestartSec=10s
    Restart=always

    [Install]
    WantedBy=default.target
```
  
  
**Starting from Shopware 6.5 , the queue has to be specifically defined and the command would change:**

```
    [Unit]
    Description=Shopware Message Queue Consumer (#%i)
    After=network-online.target
    Requires=dbus.socket  
    StartLimitIntervalSec=0

    [Service]
    Type=simple
    WorkingDirectory=%h/shopware
    ExecStart=/usr/bin/php8.1 %h/shopware/bin/console messenger:consume async --time-limit=3600 --memory-limit=2G --limit=25
    RestartSec=10s
    Restart=always

    [Install]
    WantedBy=default.target
```
**Now enable and start 2 instances for example:**
```
    systemctl --user enable shopware_consumer@{1..2}  
    systemctl --user start shopware_consumer@{1..2}
```
Restart it:
```
    systemctl --user restart shopware_consumer@*.service
```
Status:
```
    systemctl --user status shopware_consumer@*.service
```
### Disable the admin worker

If you have configured the cli-worker, you should turn off the admin worker in the shopware configuration file, therefore create or edit the config file `shopware.yaml`.
```
    # config/packages/shopware.yaml
    shopware:
        admin_worker:
            enable_admin_worker: false
```
### Magento

`~/.config/systemd/user/magento-consumer@.service`

```
    [Unit]
    Description=Magento Consumer (%i)
    After=network-online.target
    Requires=dbus.socket
    StartLimitIntervalSec=0

    [Service]
    Type=simple
    WorkingDirectory=%h/public_html
    ExecStart=php %h/public_html/bin/magento queue:consumers:start %I --single-thread --max-messages=10000
    RestartSec=10s
    Restart=always

    [Install]
    WantedBy=default.target
```

After configuring the above configuration, we use the "%I" identifier to be able to run multiple workers with just a **single systemd unit file.** 
```
    systemctl --user enable magento-consumer@sales.rule.update.coupon.usage.service
    systemctl --user start magento-consumer@sales.rule.update.coupon.usage.service
    systemctl --user status magento-consumer@sales.rule.update.coupon.usage.service
```
And then another queue , for example
```
    systemctl --user enable magento-consumer@product_action_attribute.update
    systemctl --user start magento-consumer@product_action_attribute.update
    systemctl --user status magento-consumer@product_action_attribute.update
```
Repeat this in case you want to use this for multiple queues.

Troubleshooting systemd user services
-------------------------------------

The following commands can help you troubleshoot in case the service is not working:

#### Show last 1000 lines of activity of the service:
`journalctl --user --no-pager -n 1000`
#### Target a specific user service and search for the fatal error in the last 10000 lines:  
`journalctl --user -u shopware_consumer* --no-pager -n 10000 | grep -i FATAL`
#### Follow incoming output from the log:
`journalctl --user -n 100 -f`