---
order: 190
icon: milestone
---
# DNS

The Hosted Power [customer center](https://portal.hosted-power.com/) has an application to manage DNS domains.
It can be used to manage existing domains, or new ones after registering a new [domain](./Domains.md)

!!!danger Control panels
if you already use a control panel like `DirectAdmin` or `cPanel`, we recommend using those internal DNS management tools. More information on DNS management in those control panels can be found in their own respective knowledge bases.
!!!

## Nameservers

The Hosted Power name servers:

```yaml
ns1.hosted-power.com
ns2.hosted-power.com
ns3.hosted-power.com
```
When transferring a domain, the current name servers will remain active until you manually switch to our name servers. Make sure all necessary DNS records are present before updating the name servers!

## DNS Management

1. Open the order menu
![TurboStackNewDNS](../img/customercenter/domains/cc_domain1.png)
2. Select `DNS Management` from the menu
![TurboStackNewDNS](../img/customercenter/dns/cc_dns2.png)
3. checkout the [!badge variant="success" text="FREE"] service
![TurboStackNewDNS](../img/customercenter/dns/cc_dns3.png)
4. The `DNS Management` service is now available
5. Open the management tool
![TurboStackNewDNS](../img/customercenter/dns/cc_dns4.png)
6. Add 1 domain or multiple in bulk
![TurboStackNewDNS](../img/customercenter/dns/cc_dns5.png)
7. Add the domain you already registered
8. Accept the domain
![TurboStackNewDNS](../img/customercenter/dns/cc_dns6.png)
9. New records can be added like: `A`, `AAAA`, `CNAME`, `TXT`, ...
![TurboStackNewDNS](../img/customercenter/dns/cc_dns7.png)
