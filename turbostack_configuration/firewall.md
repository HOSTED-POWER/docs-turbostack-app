---
order: 100
icon: 
---

# Firewall

Hosted Power prefers you to use our [TS CLI](../turbostack-app/turbostackcli/) commands.



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
