# Node.js Hosting Reference Guide

We provide optimized server environments for running **Node.js applications**. As the hosting provider, we maintain the server infrastructure, network, and runtime environment. Application code, dependencies, environment variables, and process management within your project areyour responsibility. (but we are always happy to assist when you are encountering issue ofcourse)

This guide outlines the typical file structure and recommended permissions for your application.

---

## File Structure

Your Node.js application typically resides in the following directories:

| Path                  | Purpose |
|-----------------------|----------|
| `~/app`               | Main Node.js application root directory. |
| `~/app/package.json`  | Project metadata and dependency definitions. |
| `~/app/node_modules`  | Installed project dependencies. |
| `~/app/public`        | Static assets (if applicable). |
| `~/app/logs`          | Application log files (if configured). |
| `~/nginx`             | Custom Nginx configuration including reverse proxy configuration. |

> Your application is typically served behind Nginx as a reverse proxy, which handles SSL termination and forwards traffic to your Node.js application.

---

## Permissions

Best practice permissions for files and directories are the following:

```bash
find ~/app -type f -exec chmod 644 {} \;
find ~/app -type d -exec chmod 755 {} \;

