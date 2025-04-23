# Managing Applications with systemd User Services

Sometimes, your application needs to be managed like a system service. Easy to start, stop, restart, and ideally run automatically on system boot or after crashes. This is where **systemd** user services come in handy.

You may already be familiar with tools like **supervisord** or **PM2**, but `systemd` is a powerful native alternative that integrates well with Linux systems, offers better logging, and doesn’t require root access when used as a user service.

This guide will walk you through the basics of setting up, troubleshooting, and applying `systemd` user services for various applications, including CMSes and frameworks.

---

## How to Set Up a systemd User Service

Setting up a `systemd` user service allows your app to automatically start on login, recover after failure, or be controlled like any other service.

### Quick Setup

Create the service file under:

```
~/.config/systemd/user/your-app.service
```

Example:

```ini
[Unit]
Description=My App Service
After=network.target
StartLimitIntervalSec=0

[Service]
ExecStart=/var/www/username/path-to-your-app/start.sh
RestartSec=10s
Restart=always
Environment=NODE_ENV=production
WorkingDirectory=/var/www/username/path-to-your-app/

[Install]
WantedBy=default.target
```

> **Hint:** `StartLimitIntervalSec=0` allows endless retrying every 10 seconds. Without it, systemd may stop trying after repeated failures.

Enable and start the service:

```bash
systemctl --user daemon-reload
systemctl --user enable your-app.service
systemctl --user start your-app.service
```

Enable lingering (start at boot even without an active login):

```bash
loginctl enable-linger your-username
```

For a deeper look: [Arch Wiki - Systemd/User](https://wiki.archlinux.org/title/Systemd/User)

---

## How to Troubleshoot a systemd User Service

`systemd` keeps logs that help determine why a service might have crashed. Use `journalctl` for this purpose.

### Show the status of the service:

```bash
systemctl --user status your-app.service
```

### View logs from the last 5 minutes:

```bash
journalctl --user -u your-app.service --since "5 minutes ago"
```

### Show the last 1000 log lines:

```bash
journalctl --user --no-pager -n 1000
```

### Search for fatal errors in a service log:

```bash
journalctl --user -u shopware_consumer* --no-pager -n 1000 | grep -i FATAL
```

### Live log feed:

```bash
journalctl --user -n 100 -f
```

---

## Examples and CMS/Framework Specific Notes

### Akeneo

**Path:** `~/.config/systemd/user/pim_job_queue@.service`

```ini
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

> **Note:** Adjust the `WorkingDirectory` to match your setup.

---

### Laravel

**Path:** `~/.config/systemd/user/laravel-queue@.service`

```ini
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

> **Note:** Adjust the PHP binary path and the `WorkingDirectory` to match your setup.

---

### CraftCMS

CraftCMS includes a queue system that handles background tasks such as image processing, email sending, and plugin actions. By default, these are run within web requests, which can cause performance issues.

A better approach is to run the queue listener as a dedicated `systemd` service.

**Path:** `~/.config/systemd/user/craftcms-queue@.service`

```ini
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

> **Note:**  
> - Disable `runqueueautomatically` in the general config settings.  
> - Adjust PHP binary path and `WorkingDirectory` as needed.

🔗 Extra Info:
- [CraftCMS Config Docs - runqueueautomatically](https://craftcms.com/docs/4.x/config/general.html#runqueueautomatically)  
- [Shopware Message Queue Guide](https://portal.hosted-power.com/knowledgebase/article/152/shopware-message-queue/)

---

### Magento

Magento uses message queues for async tasks. You can manage its consumers with `systemd`.

**Path:** `~/.config/systemd/user/magento-consumer@.service`

```ini
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

### General Information

- `%i` is a placeholder for the consumer name—reusable template
- `%h` resolves to the user’s home directory
- `--single-thread` ensures one thread per process
- `--max-messages=10000` helps restart periodically to prevent memory issues

**Examples:**

```bash
systemctl --user status magento-consumer@sales.rule.update.coupon.usage.service
systemctl --user status magento-consumer@product_action_attribute.update.service
```

You can repeat this for any other queue. Each one runs in its own isolated service.