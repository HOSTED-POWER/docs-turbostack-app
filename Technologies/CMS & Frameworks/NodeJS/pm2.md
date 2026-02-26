## Overview 

In this article we will be going over handy how-to commands in configuring, deploying and troubleshooting your PM2 application.
PM2 is a process manager for NodeJS applications, it comes with many benefits with the overall benefit being it making sure your application runs continuously and is easily manageable. 

Benefits include:
- Keeps the application running due to auto-restarts
- Managing of multiple applications
- Monitoring
- Easy viewing of logs
- ...
  
## Installation

When defining NodeJS in your deployment in my.turbostack.app pm2 will be installed by default by your node package manager :

<img width="1000" height="713" alt="image" src="https://github.com/user-attachments/assets/ac652f6a-1a9e-4824-8764-e0904a605b39" />

## Deploying the application
1. In case you migrated from another environment and you want to start with a fresh deployment, make sure to clean out the node_modules directory:

    `rm -rf node_modules`

    It could be that during migration somethig is missing/corrupted/incompatible. Removing the directory ensures a flawless reinstall.
   
2. Check the important configuration files are present and changed accordingly if needed ( `next.config.js` and `package.json`)
   
3. Install the dependencies, these will rebuild node_modules:

    `npm install`
   
4. Build the application:
   
    `npm run build`
   
5. Start the application with pm2:

    `pm2 start npm --name <appname> -- start`
  
6. As a final step do not forget to save your running configuration:

    `pm2 save`
    
  > This will store the **current** process list so the application can be restarted automatically after a reboot of the server or any other disturbance causing the service to stop. **Always do this whenever the running process list changes** ( e.g when deleting/starting a new application, changing the app name, et citera...) 
   
7.**If this is the first time setup** it could be that you need to run:

    `pm2 startup`
    
  This prepares pm2 to launch on system boot, afterwards `pm2 save` will handle all running operations.

## Monitoring and troubleshooting the application

- List all processes:

    `pm2 ls` 
  
-  Opening a live monitoring dashboard:

    `pm2 monit`
   
   This will show
     * CPU and memory usage per application
     * Application status
     * Restart count
     * Real-time logs
     
-   Display metadata of an application:

    `pm2 show <appname>`

-   Check logs of an application:

    `pm2 logs <appname>`

    There are some helpful options with this command:
      * `--lines n` : show only last n liens
      * `--err` : show only errors
      * `--out` : show only output
        
-  Restart or reload an applicaiton:

    `pm2 restart <appname>`
    
    `pm2 reload <appname>`
  
