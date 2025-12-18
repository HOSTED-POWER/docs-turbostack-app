---
hidden: true
---
# Setup example on TurboStack
In this example, we will set up a MedusaJS instance on TurboStack. This will include the demo frontend and the MedusaJS backend. 

If you only want to set up the backend, you can choose this in project creation later. MedusaJS exposes API's for you to _'Plug in'_ your own frontend.
## YAML configuration
MedusaJS requires:
- NodeJS 20+ (LTS versions only)
- PostgreSQL

The dashboard (backend) runs on port 9000/app and the demo frontend runs on port 8000.

These technologies can easily be set up on TurboStack. Just paste the following YAML configuration in the TurboStack GUI, change the values where needed (e.g. the domain names) and click **Publish**:

```yaml
webserver: nginx
postgresql_version: 18
system_users:
  - username: demo
    vhosts:
      - server_name: shopname.tld www.shopname.tld
        app_name: frontend
        nodejs_version: 24
        cert_type: letsencrypt
        proxy_enabled: true
        proxy_upstream_port: "8000"
        
      - server_name: dashboard.shopname.tld www.dashboard.shopname.tld
        app_name: dashboard
        nodejs_version: 24
        cert_type: letsencrypt
        proxy_enabled: true
        proxy_upstream_port: "9000"
```
> **Note:** If you only want to set up the backend, you can remove the frontend vhost.
---
## Server setup

### Creating the project
After the deploy succeeded, we can start setting up the server. We will use generic names, for this example. You can change these to your own.

Make a new project directory and navigate to it:
```bash
mkdir project
cd project
```
> **Important:** You don't want to place this in the `public_html` directory, as it will expose sensitive files to the public.

To create a MedusaJS project, run the following command:
```bash
npx create-medusa-app@latest <shopname>
```
This will start the interactive install process. Follow the prompts to complete the installation:
```
? Would you like to install the Next.js Starter Storefront? You can also install it later. Yes
? Enter your Postgres username demo_db
? Enter your Postgres password [hidden]
? Enter your Postgres user's database name demo_db
```
The credentials for PostgreSQL can be found in the TurboStack GUI under the **Credentials** button.

The installer is finished, when you see the following message:
```
✔ Server is ready on port: 9000
```
Once the server is ready, you can stop it with `Ctrl + C`. This is only a test environment, we'll set it up for production later.

> **Note:** If you only want the backend, say 'No' to the first prompt.

### Create an admin login
To log into the dashboard, you'll need to create an admin login. Change the values to your preference:
```bash
npx medusa user -e yourmail@tld.com -p <StrongPassword>
```
### Whitelisting your domain in MedusaJS
To whitelist your domain in MedusaJS, you'll need to add it to the `allowedHosts` array in the `medusa-config.js` file.

This file can be found at: `~/<project>/<shopname>/medusa-config.ts`. When configured, it should look like this:
```javascript
import { loadEnv, defineConfig } from '@medusajs/framework/utils'

loadEnv(process.env.NODE_ENV || 'development', process.cwd())

module.exports = defineConfig({
  projectConfig: {
    databaseUrl: process.env.DATABASE_URL,
    http: {
      storeCors: process.env.STORE_CORS!,
      adminCors: process.env.ADMIN_CORS!,
      authCors: process.env.AUTH_CORS!,
      jwtSecret: process.env.JWT_SECRET || "supersecret",
      cookieSecret: process.env.COOKIE_SECRET || "supersecret",
    },
  },

  admin: {
    // Add this block to allow your custom host in Vite dev server
    vite: (config) => {
      return {
        ...config,
        server: {
          // allow the host (and subdomains) that serve your admin dashboard
          allowedHosts: [".medusadashboard.hosted-power.dev"],
        },
      }
    },
  },
})
```
---
## Setting it up for production
The demo version is not suitable for production use. You'll need build it for production first.

### Backend
Go into the store directory `~/<project>/<shopname>/` and enter the following command:
```bash
npx medusa build
```
This should give an output like this:
```
info:    redisUrl not found. A fake redis instance will be used.
info:    Using flag MEDUSA_FF_CACHING from environment with value true
info:    Starting build...
info:    Compiling backend source...
info:    Removing existing ".medusa/server" folder
info:    Compiling frontend source...
info:    Backend build completed successfully (2.97s)
info:    Frontend build completed successfully (16.17s)
```
Change into `~/<project>/<shopname>/.medusa/server` and install the dependencies:
```bash
npm install
```
Now copy over the environment file from the medusa project root directory:
```bash
cp ../../.env .env
```
We can now try to start the server with:
```bash
npx medusa start
```
Now we can test if the backend loads by opening `https://dashboard.shopname.tld` in your browser.
If the backend is confirmed to work, we can now set up the PM2 user service for it with the following command:
```bash
pm2 start "npx medusa start"  --name medusa-backend
```
---
### Frontend (Optional)
To get the storefront to run, cd into `~/<project>/<shopname>-storefront` and install npm dependencies:
```bash
npm install
```
Build it:
```bash
npm run build
```
Start it:
```bash
npm start
```
This should give an output like:
```
> medusa-next@1.0.3 start
> next start -p 8000

   ▲ Next.js 15.3.6
   - Local:        http://localhost:8000
   - Network:      http://85.9.219.32:8000

 ✓ Starting...
 ✓ Ready in 240ms
```
If the frontend is confirmed to work, we can CTRL+C to stop it, and set up the PM2 user service for it with the following command:
```bash
pm2 start "npm start"  --name storefront
```
---
## Tips
### Automating the deployment process
If you want to automate the deployment process, you can do two things:
  - Include the database connectionstring in the create-medusa-app command
  - use the `--with-nextjs-starter` flag to include the storefront, then delete it after the install.
  - Grab the Postgres credentials on the server, instead of the TurboStack GUI.

To get the Postgres credentials, run the following command in the home directory of the user:
```bash
cat .pgpass
```

The command should look like this:
```bash
npx create-medusa-app@latest shopname --db-url "postgres://<username>:<password>@localhost:5432/<dbname>" --with-nextjs-starter
```

After the install, delete the storefront directory.
---
