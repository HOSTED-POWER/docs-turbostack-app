---
order: 220
icon: flame
---
# Firewall

## Firewall Brute Force Protection

It's possible you, certain API calls or your customers get blocked by the security mechanisms of our Firewall. The "Login Failure Daemon" component of our firewall could block too many failed login attempts to SSH, FTP, POP3, IMAP, HTTP, etc.

Unlike other applications, LFD is a daemon process that monitors logs continuously and so can react within seconds of detecting such attempts. It also monitors across protocols, so if attempts are made on different protocols in a short space of time, all those attempts will be counted against the threshold.

Normally the protection is configured so it should not generate false positives easily. However you can whitelist your known ip addresses if needed trough the TurboStack App or via the ts-cli as explained below.

## Firewall CLI on TurboStack®

Hosted Power prefers you to use our [TS CLI](https://docs.turbostack.app/turbostack-app/turbostackcli/) commands if you want to make changes to the Firewall settings.

```
Usage: tscli firewall [OPTIONS] COMMAND [ARGS]...
 
  Firewall service - run 'tscli firewall' to see all possible commands
 
Options:
  --help  Show this message and exit.
 
Commands:
  block      Add an IP to the blocklist.
  check      Lookup an IP in the blocklist.
  flush      Flush all automatic blocks from the firewall rules.
  unblock    Remove an IP from the blocklist.
  unlist     Remove the provided IP from both allow and ignore lists
  whitelist  Add the provided IP to both allow and ignore lists
```

### Whitelist an IP address in the TurboStack® App

If you want to completely whitelist an ip address this, it could be done trough the TurboStack® app. A whitelisted ip address will never be blocked by the brute force protections.

Furthermore it would allow access to ports which are otherwise closed (blocked) by the firewall. So it could be used to allow accessing certain restricted ports like for MySQL and other databases (Direct external access).

![Whitelist Ip Address trough the TurboStack App](image/firewall/1715869950499.png "Whitelist Ip Address trough the TurboStack App")

## Control Panels with Firewall GUI

If you're using a control panel like [DirectAdmin](../Technologies/Control%20Panels/directadmin.md) or [cPanel](../Technologies/Control%20Panels/cpanel.md), you'll also be able to use the Firewall GUI.

### Unblock an IP Address in Configserver & Reason for blocking

 This can be done via:

Home » Plugins » ConfigServer Security & Firewall

To determine whether an IP address is blocked, you can use the Search for IP button on the ConfigServer Security&Firewall page. Simply enter the IP address into the search field and click the button.

If the IP address is blocked, the reason for the block will be listed and an unlocked padlock icon will appear to the right of the blocked IP address. Clicking the padlock icon will unblock the IP in the firewall.

### Whitelist an IP address in CSF

It's possible to ignore some IP-Addresses for your office, home office or third party that are working for something like testing the website/application or if you running a vulnerability scan. Sometimes it can be happy that you will be blocked by the CSF for many failed logins how we explained here above the article. Ignoring an IP-address will be a good solutions to avoid blocking.

Ignoring (Whitelisting) an IP Address
Home »Plugins »ConfigServer Security & Firewall

To ignore an IP address in the firewall (csf.ignore), you can enter the IP address into the Quick Ignore section,  allow such as “Office network”, and click the Quick Ignore button.
