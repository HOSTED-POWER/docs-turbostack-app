---
hidden: true
---
# Odoo Hosting Reference Guide

As part of your hosting environment with Hosted Power, your Odoo application is managed via a `systemd` service under your Linux user account. This gives us robust process management, automatic restarts, and easy logging—without requiring containerization or root-level access.

This guide will walk you through the key components of the Odoo service setup, application file structure, and performance tuning.

---

## Systemd Service Overview

We use a systemd **user-level service** to manage and monitor your Odoo application. The service configuration is stored at:

```bash
~/.config/systemd/user/application.service
```

### Example systemd service file:

```ini
[Unit]
Description=Odoo application (main) for %u
Wants=network-online.target
After=network-online.target
Requires=dbus.socket
StartLimitIntervalSec=0

[Service]
LimitNOFILE=819200
LimitNPROC=819200
LimitMEMLOCK=infinity

TimeoutStartSec=900
Type=simple
WorkingDirectory=%h/application/odoo
EnvironmentFile=%h/conf/.env

ExecStart=%h/.pyenv/shims/python %h/application/odoo/odoo-bin -c %h/conf/odoo.conf
ExecStop=/bin/kill -s TERM $MAINPID
Restart=on-failure
RestartSec=10s
KillSignal=SIGQUIT
StandardOutput=append:%h/logs/odoo.log

[Install]
WantedBy=default.target
```

## File structure

The service assumes the following directory layout under your home directory:

| Path                 | Purpose                                     |
| -------------------- | ------------------------------------------- |
| `~/application/odoo` | Odoo source code and your working directory |
| `~/conf/odoo.conf`   | Main Odoo configuration file                |
| `~/conf/.env`        | Environment variables loaded into systemd   |
| `~/logs/odoo.log`    | Application logs (append-only)              |

### Custom modules

To add custom modules, place them in:

```bash
~/application/odoo/addons
```

You can also create symbolic links inside the addons directory to keep your structure modular.

!!! Important
To safely test new modules or updates, use a separate **staging server**. Do **not** run staging environments locally on the same production host.

Why Separate Servers?
* Prevents production disruption.
* Isolates tests and experiments.
* Maintains better performance and security.
!!!

### Odoo.conf tuning

You can find your configuration at:

```bash
~/conf/odoo.conf
```

Here are some general tuning recommendations for various server sizes:

#### 8 GB RAM / 4 CPU Cores:

``` ini
workers = 8
limit_memory_hard = 4294967296        ; 4 GB
limit_memory_soft = 2147483648        ; 2 GB
limit_request = 8192
limit_time_cpu = 3600
limit_time_real = 7200
limit_time_real_cron = 0
max_cron_threads = 2
proxy_mode = True
```

#### 48 GB RAM / 12 CPU Cores

``` ini
workers = 36
limit_memory_hard = 6442450944        ; 6 GB
limit_memory_soft = 4294967296        ; 4 GB
limit_request = 8192
limit_time_cpu = 3600
limit_time_real = 7200
limit_time_real_cron = 0
max_cron_threads = 4
proxy_mode = True
```

## Logs

Odoo logs are continuously appended to:

```bash
~/application/odoo/addons
```

Monitor this file to troubleshoot startup or runtime issues.

## Configuration Key Reference

| Key                    | Description                                                                           |
| ---------------------- | ------------------------------------------------------------------------------------- |
| `workers`              | Set to 2×CPU by default. On high-demand systems, 3–4×CPU may be used.                 |
| `limit_memory_hard`    | Upper memory cap per worker. Set around **½ total RAM**.                              |
| `limit_memory_soft`    | Warning memory cap per worker. Set around **¼ total RAM**.                            |
| `limit_request`        | Number of requests a worker handles before restarting. Default 8192 is fine for most. |
| `limit_time_cpu`       | Max CPU time per request. `3600` (1 hour) ensures long operations are still capped.   |
| `limit_time_real`      | Wall-clock time per request. `7200` (2 hours) is generous and rarely hit.             |
| `limit_time_real_cron` | Time limit for cron jobs. `0` means unlimited. Can be set to `86400` (24h).           |
| `max_cron_threads`     | Limit to about **⅓ of CPU cores**, up to ½ in large setups.                           |
| `proxy_mode`           | Always set `True` if behind a reverse proxy (e.g. nginx, traefik).                    |
| `db_maxconn`           | Leave at default (`64`). Our PostgreSQL is tuned for high concurrency.                |

## Best practices

* Keep your application/odoo directory clean and structured.
* Use symbolic links for large or custom addon libraries.
* Don’t over-allocate memory or workers unless necessary—we monitor and optimize for you.
* Always keep your .env and odoo.conf safe and backed up.

## Need help?

Contact our support if you're unsure about config tuning, migration, or if you experience performance bottlenecks. We’re here to assist with your Odoo stack.

