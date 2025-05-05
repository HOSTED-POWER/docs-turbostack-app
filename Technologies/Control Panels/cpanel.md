---
order: 100
icon: sliders
---

# cPanel

## Troubleshoot failed AutoSSL/Let's Encrypt certificate request

If you notice issues with one or more domains not getting their certificates, you probably already know to check the 'AutoSSL' page to retry. There's also a way to check how the failed request went on [this page](https://web2.monkeyhost.be:2087/cpsess2457181835/scripts12/autossl#/providers/) though, and often you can easily identify the issues.

We strongly recommend retrying the request only once, for the specific user that is experiencing the issue, and then fixing the issue using that log. Further retries will count towards the Let's Encrypt request limit, which will block further requests eventually.

Reading through this log may seem daunting, as there is a lot of information especially for users with a lot of domains. Most of it can be safely ignored however, we're mostly looking for warnings and errors. When going down the list, you'll find 'HTTP DCV' mentioned. This means 'domain control validation', or the validation that you own the domain you're requesting a certificate for, over HTTP. If you find 'WARN Local HTTP DCV error' for a specific domain, said validation did not succeed for this domain. Take a look at the example below:

```
 WARN Local HTTP DCV error (www.hosted-power.com): The system failed to fetch the DCV (Domain Control Validation) file at “http://www.hosted-power.com/.well-known/acme-challenge/C8FHCBYKJ8VMHGMNAQDWOYK9T3KU11AO” because of an error: The system failed to send an HTTP (Hypertext Transfer Protocol) “GET” request to “http://www.hosted-power.com/.well-known/acme-challenge/C8FHCBYKJ8VMHGMNAQDWOYK9T3KU11AO” because of an error: Timed out while waiting for socket to become ready for reading. The domain “www.hosted-power.com” resolved to an IP address “94.237.46.68” that does not exist on this server.
```
This error tells us that Let's Encrypt was not able to reach the validation file (the URL provided) and the reason why. In this case, we can see the domain resolved to an IP address that cPanel could not find on the server, indicating the domain does not point towards the server we're requesting the certificate on. So we could resolve this either by fixing the DNS records to point towards this server, or we can [exclude this domain](./cpanel.md/#fix---autossl-reduced-ssl-coverage) from AutoSSL for this user.

Other common errors are a 404 response, indicating that the site may resolve to this server correctly, but the file is still not reachable. This could be due to redirections you have setup, or even a faulty Magento runmaps setup. 

## How to correctly configure a Magento 2 cronjob under cPanel

Login via ssh as the account user (or login via cPanel -> Cronjobs)

```
# crontab -e
MAILTO=""
* * * * * /usr/local/bin/php -d memory_limit=-1 /home/demoshop/public_html/bin/magento cron:run > /home/demoshop/public_html/var/log/magento.cron.log
* * * * * /usr/local/bin/php -d memory_limit=-1 /home/demoshop/public_html/update/cron.php > /home/demoshop/public_html/var/log/update.cron.log
* * * * * /usr/local/bin/php -d memory_limit=-1 /home/demoshop/public_html/bin/magento setup:cron:run > /home/demoshop/public_html/var/log/setup.cron.log
```
(Replace demoshop by your own account name)

## FIX - AutoSSL reduced SSL coverage

This is a known problem in cPanel AutoSSL.
 
This now requires each subdomain be verified individually. If this fails, they will automatically stop including them in the attempt and send an alert to your configured mail addresses. This can get very spammy, so you might want to solve the root cause. You can find more context in [the official cPanel docs](https://support.cpanel.net/hc/en-us/articles/4416419981335-Potential-reduced-AutoSSL-coverage-notification). 

To resolve this, we recommend you check the configured domains. As you can see in the screenshot below, this often includes auto-generated cPanel subdomains

![AutoSSL issue](../../img/turbostackapp/control_panels/kb-cpanel-autossl-issue1.png)

As you can see, there's a lot of unprotected subdomains, however, none of these are actually in use. As such, you can safely exclude these from your certificate.
To do this, you need to be logged in as the user of the domain for which the certificate is being generated.

Once logged in, you need to find the following option in your main menu: SSL/TLS Status 

That page will look like this:
![AutoSSL issue2](../../img/turbostackapp/control_panels/kb-cpanel-autossl-issue2.png)

Here you can see what your subdomains might look like. "AutoSSL Domain Validated" means it works, so you can ignore those. Then, all subdomains in red need to be checked. If you don't use the subdomain, you should click "Exclude from AutoSSL" as seen in the second item in the screenshot. 
Once you've dealt with all subdomains in error, you can click the "Run AutoSSL" button at the top of the page to re-run the validation. This will mean no more errors when running AutoSSL, and no more alert spam!
