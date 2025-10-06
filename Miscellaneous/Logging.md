In this documentation we will show you where the logs are, how to access them, and how to interpret them.

We rotate the logs daily and compress the older ones to save on storage.

Depending on the service, your access might be limited to the logs of the service running on that account. For example, the PHP logs for the website running under a certain account, but not the PHP version logs that contains information about other environments.

| Action               | Default                          | Notes                                             |
| -------------------- | -------------------------------- | ------------------------------------------------- |
| Rotation schedule    | Daily at 00:00 server time       | Managed by **logrotate** via `/etc/logrotate.d/*` |
| Uncompressed history | Today & yesterday (`*.log`)      | Consult logs with `cat` command.                  |
| Compressed history   | Older than 2 days (`*.log.2.gz`) | Consult logs with `zcat` command.                 |
| Retention            | 30 days                          | Sometimes combined in monthly log                 |

**Note:** Some services deviate from the 30-day policy to conserve diskspace.  
**Note:** DirectAdmin and cPanel keep today's logs easily accessable. After the day has ended, it is consolidated into the monthly logfile.

### Turbostack

The logs for all the services are located in `/var/log/servicename`.

#### Nginx
When switching to `/var/log/nginx`, you'll find:  

- All the logs for the different environments.  
- An **error.log** file containing extra helpful information.  
- An **access.log** file containing all the requests not linked to a vhost.  

You can access the logs of all environments via one account.

#### Apache2
Depending on the age of your setup, the logs will be located in:

- `/var/log/apache` or `/var/log/apache/domlogs/yourSite`.  
- `/var/log/httpd` or `/var/log/httpd/domlogs/yoursite`.  

Just like nginx, you'll find an **error.log** and **access.log** file to provide some extra information.  

**Access.log** will contain all requests that could not be linked to a vhost on the server.

All the logs in this folder are also accessible via any system account.

---
### DirectAdmin
DA provides a handy interface for us to view the logs at the following locations:

#### Admin level
- Navigate to **Admin Tools > Log Viewer**
- Lets you view general service logs (Apache, Nginx, Exim, system messages).
#### User level
- Navigate to **User Tools > Site Summary / Statistics / Logs**
- This will give you the Apache logs for today
- `Backed up Web Logs` opens a file viewer showing older, compressed logs

---
### cPanel

cPanel provides a **web interface** for accessing logs. Direct access to system log files (like Apache’s `/usr/local/apache/logs/error_log`) is typically restricted to root users, and **not available via SSH** on shared hosting.

The following logs can be consulted via the user account, not via the Admin.
#### Error Logs
- Accessible via **cPanel > Metrics > Errors**
- Displays the most recent error log entries for your domain.
- Useful for debugging PHP errors, missing files, or permission issues.
#### Raw Access Logs
In this overview, you can download both the access logs from today, and the aggregated logs per month. The logs will be downloaded in a .gz file.

- Found under **cPanel > Metrics > Raw Access**
- Provides access logs for your domains.
- You can **download raw log files** for offline analysis.
- Logs are rotated daily; older ones are compressed.

#### Awstats / Webalizer (Traffic Analysis)
- Found under **cPanel > Metrics > Awstats** (or Webalizer, depending on setup).
- Offers a graphical interface for analyzing traffic, referrers, bots, and bandwidth usage.
#### Email Logs
While raw mail logs aren’t directly exposed, cPanel provides interfaces under **Email** (Email Deliverability, Track Delivery).

For detailed troubleshooting, use **Track Delivery**:
- **cPanel > Email > Track Delivery**
- Shows delivery attempts, successes, and failures.

---
### How to search efficiently in the logs
Once you have the raw logs downloaded, you can do a quick analysis on them. Here are some quick commands that can help you.

#### Quick one liners

Show the top 5 IP addresses with the most requests:
```bash
awk '{print $1}' logname.log | sort | uniq -c | sort -nr | head -5
```

Show the top 5 paths:
```bash
awk '{print $7}' logname.log | sort | uniq -c | sort -nr | head -5
```

Show total requests per status code:
```bash
awk '{print $9}' logname.log | sort | uniq -c | sort -nr
```

Show the most common User Agents:
``` bash
awk -F\" '{print $6}' logname.log | sort | uniq -c | sort -nr | head -5
```

#### Combining the info
For example: You notice a lot of error 403's in the logs. This could be an indication of scraping or even an attempt to find a way to exploit a fault.

In this case, we can find the IP's causing the most of these errors:
```bash
grep "403" logname.log | awk '{print $1}' | sort | uniq -c | sort -nr | head -5
```
---
