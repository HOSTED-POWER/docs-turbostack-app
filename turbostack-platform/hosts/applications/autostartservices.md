## Self-restarting Services
It's critical to make sure your services always restart without intervention. This ensures that your application will be available again after a reboot or unexpected shutdown.

!!!
Important note: Hosted Power cannot guarantee the availability or uptime of your applications if the (re)start of your services are not configured correctly!
!!!

Depending on the technology your application uses, there are different ways to achieve this. Below are the instructions for the technologies supported by TurboStack:

### Systemd User Service
For PHP frameworks like Laravel, Magento and Akeneo, you can use systemd user services to automatically restart your application.

You can find the docs [here](../../../Miscellaneous/systemd.md "Systemd User Service").

### PM2
For Node.js based applications, you can use PM2 to automatically restart your application.

You can find the docs [here](../../../Technologies/CMS%20&%20Frameworks/Node.js/pm2.md "PM2").

### Docker
Docker offers integrated support for automatic restarts.

You can find the docs [here](../../../Technologies/Containerization/Docker/referenceguide.md "Docker Reference Guide").
