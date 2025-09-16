---
order: 220
icon: flame
---
# Firewall

## Firewall Brute Force Protection

Your access, certain API calls, or your customers may be blocked by our firewall's security mechanisms. The Login Failure Daemon (LFD) component actively monitors failed login attempts across multiple protocols, including SSH, FTP, POP3, IMAP, and HTTP, and may block access after too many failures.

Unlike other applications, LFD operates as a continuous monitoring daemon, reacting within seconds to suspicious activity. It also tracks failed attempts across different protocols, counting them collectively against the security threshold.

While the protection is designed to minimize false positives, you can whitelist trusted IP addresses if needed. This can be done easily via the TurboStack App or the TurboStack CLI, as outlined below.

## Firewall CLI on TurboStack®

Hosted Power prefers you to use our [Turbostack CLI](https://docs.turbostack.app/turbostack-app/turbostackcli/) commands if you want to make changes to the Firewall settings.

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

To completely whitelist an IP address, you can use the TurboStack App. A whitelisted IP address will never be blocked by brute force protection.

Additionally, whitelisting an IP grants access to ports that are otherwise blocked by the firewall. This can be useful for enabling remote access to MySQL and other databases or allowing connections to restricted services.

Firewall settings can be managed in the Security tab of your host's management section within the [TurboStack App](https://my.turbostack.app "TurboStack App"). For detailed instructions on modifying firewall settings, please refer to the documentation [here](https://docs.turbostack.app/turbostack-app/overview/#security-tab "Here").

## Control Panels with Firewall GUI

If you’re using a control panel like [DirectAdmin](../Technologies/Control%20Panels/directadmin.md) or [cPanel](../Technologies/Control%20Panels/cpanel.md), you can also manage firewall settings directly through their graphical user interface (GUI). For detailed instructions, please refer to their respective knowledge bases.
