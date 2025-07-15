---
hidden: true
---
## Overview
Upgrade or install Odoo modules using command-line tools. Our servers provide optimal system configurations, but managing Odoo itself remains your responsibility.

!!! Important
To safely test new modules or updates, use a separate **staging server**. Do **not** run staging environments locally on the same production host.

Why Separate Servers?
* Prevents production disruption.
* Isolates tests and experiments.
* Maintains better performance and security.
!!!

## Upgrade an Addon

```
systemctl --user stop application.service
~/application/odoo/odoo-bin -u <module_name> -c ~/conf/odoo.conf --stop-after-init
systemctl --user start application.service
```

Replace `<module_name>` with your actual addon/module name.

## Install a New Addon

1. Upload the module to `~/application/addons/`.
2. Run:

```
systemctl --user stop application.service
~/application/odoo/odoo-bin -i <module_name> -c ~/conf/odoo.conf --stop-after-init
systemctl --user start application.service
```

## Notes
- Always stop the service before upgrades.
- Make sure your addon contains a valid `__manifest__.py` file.
