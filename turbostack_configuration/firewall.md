---
order: 100
icon: flame
---

# Firewall

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

## ConfigServer Security & Firewall cheat sheet

Useful CSF SSH Command Line Commands in a “cheat sheet” format.

Command | Description | Example
--- | --- | ---
csf -s | Start the firewall rules | root@server[~]#csf -s
csf -f | Flush/Stop firewall rule (note: lfd may restart csf) | root@server[~]#csf -f
csf -r	| Restart the firewall rules	| root@server[~]#csf -r
csf -a | [IP.add.re.ss] [comment]	Allow an IP(allow access to all ports) and add to /etc/csf/csf.allow | root@server[~]#csf -a 187.33.3.3 Home IP Address
csf -tr | [IP.add.re.ss]	Remove an IP from the temporary IP ban or allow list | root@server[~]#csf -tr 66.192.23.1
csf -tf |	Flush all IPs from the temporary IP entries | root@server[~]#csf -tf
csf -d | [IP.add.re.ss] [comment]	Deny an IP and add to /etc/csf/csf.deny |	root@server[~]#csf -d 66.192.23.1 Blocked This Guy
csf -dr | [IP.add.re.ss]	Unblock an IP and remove from /etc/csf/csf.deny |	root@server[~]#csf -dr 66.192.23.1
csf -df	| Remove and unblock all entries in /etc/csf/csf.deny	| root@server[~]#csf -df
csf -g | [IP.add.re.ss]	Search the iptables and ip6tables rules for a match (e.g. IP, CIDR, Port Number) |	root@server[~]#csf -g 66.192.23.1
csf -t |	Displays the current list of temporary allow and deny IP entries with their TTL and comment	| root@server[~]#csf -t
csf -e | Enable CSF Firewall
csf -x | Diable CSF Firewall
csf -ra | Restart CSF/LFD Firewall (iptables rules and LFD service)
service lfd restart | Restart LFD only
csf -td IP 86400 | Block IP (temporarily for 24 hours, define in seconds) | 
csf -a IP | Whitelist IP (allow access to all ports)
csf -a 192.168.0.0/24 | Whitelist (temporarily) IP range /24 (allow access to all ports)
csf -ta 192.168.0.0/24 86400 | Whitelist (temporarily) IP range /24 for 24 hours (allow access to all ports, define in seconds)
csf -tf | Remove all temporary IP blocks

Configuration location is in the folder /etc/csf/
Main configuration file: /etc/csf/csf.conf


## CSF Login Failure Daemon (LFD) blocks
It's possible you or your customers get blocked by the security mechanisms of the ConfigServer Firewall. The LFD component could block too many failed login attempts to SSH, FTP, POP3, IMAP, HTTP, etc.

Unlike other applications, LFD is a daemon process that monitors logs continuously and so can react within seconds of detecting such attempts. It also monitors across protocols, so if attempts are made on different protocols in a short space of time, all those attempts will be counted against the threshold.

### Unblock an IP Address in CSF & Reason for blocking

Unblocking an IP Address in CSF & Reason of blocking
Home » Plugins » ConfigServer Security & Firewall

To determine whether an IP address is blocked, you can use the Search for IP button on the ConfigServer Security&Firewall page. Simply enter the IP address into the search field and click the button.

If the IP address is blocked, the reason for the block will be listed and an unlocked padlock icon will appear to the right of the blocked IP address. Clicking the padlock icon will unblock the IP in the firewall.

### Whitelist an IP address in CSF

It's possible to ignore some IP-Addresses for your office, home office or third party that are working for something like testing the website/application or if you running a vulnerability scan. Sometimes it can be happy that you will be blocked by the CSF for many failed logins how we explained here above the article. Ignoring an IP-address will be a good solutions to avoid blocking. 

Ignoring (Whitelisting) an IP Address
Home »Plugins »ConfigServer Security & Firewall

To ignore an IP address in the firewall (csf.ignore), you can enter the IP address into the Quick Ignore section,  allow such as “Office network”, and click the Quick Ignore button.
