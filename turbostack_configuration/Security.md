---
order: 180
icon: key
---

# Security

It is essential that companies take the security of their application seriously; as data breaches can have serious and long-lasting consequences. 
Malicious attacks can cause your website or application to be temporarily or permanently disabled. 
It can cause significant financial damage to your business, and damage to the trust of your customers if their personal information is released through your site or application. That is why, we at **Hosted Power** are taking the necessary steps to help prevent this. 

## Virtual Access Security (SSL/TLS) 

Virtual access protection ensures that if someone tries to intercept data while it is being sent over the internet, they will only see unreadable, unintelligible characters.
SSL/TLS-encryption is such an integral part of website security, especially for e-commerce sites. **Hosted Power** provides this service for free. Not only does this help protect your customers, but search engines are increasingly labelling websites without SSL/TLS certificates as “insecure”, driving visitors away. 

You can find more information on SSL/TLS on our dedicated page on the topic.

## Managed Backup

Backups are of course important, because in the event that your website or application crashes or gets compromised, you don’t want to lose all your data and have to rebuild your environment from scratch. **Hosted Power** provides a managed backup solution within the Service Level Agreement (SLA). By default, the backups are kept for 20 days and will be backed up daily. This retention period can be customized if necessary and is included in the Platinum SLA.

!!! Important
Any restores are included in the price and service. The time needed to restore is outside the SLA time. 
!!!

## DDoS-prevention

Distributed Denial-of-Service (DDoS)-attacks are unfortunately a common tool in hacker’s arsenal. In a DDoS attack, they flood your environment with so much traffic that it creates a traffic jam and makes your application inaccessible to legitimate users, leaving your customers ‘deprived of service” as the webserver no longer can accept it. Since DDos attacks can be difficult to resolve, it is important to prevent them before they can happen. **Hosted Power** takes several steps at the network level to fend off these attacks. Here is a summary: 

### Rate limiting 
By limiting the number of connections a single client can open in a certain amount of time can preventively limit potential DDoS attacks. In normal use, a web browser can open 5 to 7 TCP connections to a single website when all the resources are loaded to display the page. In contrast, DDoS attacks often go much further than this to maximize their effect. As such, anything above 10 connections simultaneously can be consider unusual activity.

### Load balancing
For rate throttling, a load balancer can be a useful first line of defense against DDoS. For example, HAProxy, which is primarily a load balancer proxy for TCP and HTTP, can also act as a traffic controller. HAProxy can be used to protect against DDoS attacks by denying or redirecting connections based on various identifiers such as IP, URL or cookies.

### Blackhole-routing
One of the simplest countermeasures to mitigate a DDoS attack is to divert the flow of connections in to a “black hole” b throwing all the data away. Optional extra DDoS-security The most effective way to mitigate DDoS attacks is simply to have more capacity to process incoming data than the cybercriminals can muster. It is unlikely that a single server or service can achieve this on its own, which is why a helping hand from a third party DDoS protection service, such as Cloudflare, can be extremely helpful.

## WAF Security

**Hosted Power** obviously takes security into account and can provide Web Application Firewall (WAF) based on [ModSecurity](https://owasp.org/www-project-modsecurity/). If desired, in addition to ModSecurity, Fortinet FortiWeb Application Firewall can also be set up. This is considered case by case. A WAF goes much further than a standard firewall by analyzing the application layer (OSI layer 7) instead of stopping at Layer 3. The WAF offers, amongst other things, extra protection against SQL injection, XSS attacks, etc. According to the [OWASP top 10](https://owasp.org/www-project-top-ten/).

The WAF contains virtual patches for a kinds of frameworks. This guarantees extra high security out of the box, because this way an (for example recently) unpatched leak can still be closed immediately before a patch has been applied to the application layer. If desired, Hosted Power has a WAF available. Please contact your personal account manager if you would like more information about this subject. 

