---
order: 200
icon: mail
---

# Mail

## Mail delivery best practices

* SPF record
  * Most important, make sure to add both the ipv4 and ipv6 of your server!!
  * Use -all at the end of your record!
* DKIM will enhance delivery, however SPF will give you the most gain in delivery success.
* DMARC is an extra record which can improve even more, but it's gain in delivery success is less than SPF and DKIM.
  * However it's quite important from a security point of view (avoid abuse by external parties).
  * The most basic DMARC txt record to create, which will enforce SPF: v=DMARC1; p=reject; aspf=s;
* In doubt: Always use [Mail tester](https://www.mail-tester.com/)
  * Make sure that you use the exact same method as the one your experience problems with when sending the mail to mail-tester.

## SPF Record Syntax

### Mechanisms
Mechanisms can be prefixed with one of four qualifiers:

* "+"	Pass
* "-"	Fail
* "~"	SoftFail
* "?"	Neutral

If a mechanism results in a hit, its qualifier value is used. The default qualifier is "+", i.e. "Pass". For example:
```
"v=spf1 -all"
"v=spf1 a -all"
"v=spf1 a mx -all"
"v=spf1 +a +mx -all"
```
Mechanisms are evaluated in order. If no mechanism or modifier matches, the default result is "Neutral".

If a domain has no SPF record at all, the result is "None". If a domain has a temporary error during DNS processing, you get the result "TempError" (called "error" in earlier drafts). If some kind of syntax or evaluation error occurs (eg. the domain specifies an unrecognized mechanism) the result is "PermError" (formerly "unknown").

Evaluation of an SPF record can return any of these results:
```
Result	  Explanation	                                                                              Intended action
Pass	    The SPF record designates the host to be allowed to send	                                        accept
Fail	    The SPF record has designated the host as NOT being allowed to send	                                reject
SoftFail  The SPF record has designated the host as NOT being allowed to send but is in transition	accept but mark
Neutral	  The SPF record specifies explicitly that nothing can be said about validity	                        accept
None	    The domain does not have an SPF record or the SPF record does not evaluate to a result   	        accept
PermError	A permanent error has occured (eg. badly formatted SPF record)	                                 unspecified
TempError A transient error has occured	                                                                      accept or reject
```

### The "all" mechanism 
```
all
```
This mechanism always matches. It usually goes at the end of the SPF record.

Examples:
```
"v=spf1 mx -all"
```
Allow domain's MXes to send mail for the domain, prohibit all others.
```
"v=spf1 -all"
```
The domain sends no mail at all.
```
"v=spf1 +all"
```
The domain owner thinks that SPF is useless and/or doesn't care.
 

### The "ip4" mechanism
```
ip4:
ip4:/
```
The argument to the "ip4:" mechanism is an IPv4 network range. If no prefix-length is given, /32 is assumed (singling out an individual host address).

Examples:
```
"v=spf1 ip4:192.168.0.1/16 -all"
```
Allow any IP address between 192.168.0.1 and 192.168.255.255.
 

### The "ip6" mechanism 
```
ip6:
ip6:/
```
The argument to the "ip6:" mechanism is an IPv6 network range. If no prefix-length is given, /128 is assumed (singling out an individual host address).

Examples:
```
"v=spf1 ip6:1080::8:800:200C:417A/96 -all"
```
Allow any IPv6 address between 1080::8:800:0000:0000 and 1080::8:800:FFFF:FFFF.
```
"v=spf1 ip6:1080::8:800:68.0.3.1/96 -all"
```
Allow any IPv6 address between 1080::8:800:0000:0000 and 1080::8:800:FFFF:FFFF.
 

### The "a" mechanism 
```
a
a/
a:
a:/
```
All the A records for domain are tested. If the client IP is found among them, this mechanism matches. If the connection is made over IPv6, then an AAAA lookup is performed instead.

If domain is not specified, the current-domain is used.

The A records have to match the client IP exactly, unless a prefix-length is provided, in which case each IP address returned by the A lookup will be expanded to its corresponding CIDR prefix, and the client IP will be sought within that subnet.
```
"v=spf1 a -all"
```
The current-domain is used.
```
"v=spf1 a:example.com -all"
```
Equivalent if the current-domain is example.com.
```
"v=spf1 a:mailers.example.com -all"
```
Perhaps example.com has chosen to explicitly list all the outbound mailers in a special A record under mailers.example.com.
```
"v=spf1 a/24 a:offsite.example.com/24 -all"
```
If example.com resolves to 192.0.2.1, the entire class C of 192.0.2.0/24 would be searched for the client IP. Similarly for offsite.example.com. If more than one A record were returned, each one would be expanded to a CIDR subnet.
 

### The "mx" mechanism
```
mx
mx/
mx:
mx:/
```
All the A records for all the MX records for domain are tested in order of MX priority. If the client IP is found among them, this mechanism matches.

If domain is not specified, the current-domain is used.

The A records have to match the client IP exactly, unless a prefix-length is provided, in which case each IP address returned by the A lookup will be expanded to its corresponding CIDR prefix, and the client IP will be sought within that subnet.

Examples:
```
"v=spf1 mx mx:deferrals.domain.com -all"
```
Perhaps a domain sends mail through its MX servers plus another set of servers whose job is to retry mail for deferring domains.

```
"v=spf1 mx/24 mx:offsite.domain.com/24 -all"
```
Perhaps a domain's MX servers receive mail on one IP address, but send mail on a different but nearby IP address.
 
### The "include" mechanism
```
include:
```
The specified domain is searched for a match. If the lookup does not return a match or an error, processing proceeds to the next directive. Warning: If the domain does not have a valid SPF record, the result is a permanent error. Some mail receivers will reject based on a PermError.

Examples:

In the following example, the client IP is 1.2.3.4 and the current-domain is example.com.
```
"v=spf1 include:example.com -all"
```
If example.com has no SPF record, the result is PermError.
Suppose example.com's SPF record were "v=spf1 a -all".
Look up the A record for example.com. If it matches 1.2.3.4, return Pass.
If there is no match, other than the included domain's "-all", the include as a whole fails to match; the eventual result is still Fail from the outer directive set in this example.
Trust relationships — The "include:" mechanism is meant to cross administrative boundaries. Great care is needed to ensure that "include:" mechanisms do not place domains at risk for giving SPF Pass results to messages that result from cross user forgery. Unless technical mechanisms are in place at the specified otherdomain to prevent cross user forgery, "include:" mechanisms should give a Neutral rather than Pass result. This is done by adding "?" in front of "include:". The example above would be:
```
"v=spf1 ?include:example.com -all"
```
In hindsight, the name "include" was poorly chosen. Only the evaluated result of the referenced SPF record is used, rather than acting as if the referenced SPF record was literally included in the first. For example, evaluating a "-all" directive in the referenced record does not terminate the overall processing and does not necessarily result in an overall Fail. (Better names for this mechanism would have been "if-pass", "on-pass", etc.)

## Common SMTP and E-mail ports

Common SMTP ports:
* SMTP – port 25 or 587
* Secure SMTP (SSL / TLS) – port 465 or 587

Common Email retrieval ports:
* POP3 – port 110
* POP3 SSL (POP3S) – port 995
* IMAP – port 143
* IMAP SSL (IMAPS) – port 993

## SPF & DKIM Record For MailChimp 
To authenticate your domain, you'll need to complete tasks in Mailchimp and in the zone editor(portal) or control panel. This process requires you to copy and paste information from Mailchimp to your domain provider's site. We recommend that you work with two browser windows or tabs to easily move between your sites.

Here's a brief overview of the process:

In Mailchimp
* Verify your domain.
* Copy two important pieces of information, your CNAME record for DKIM, and your TXT record for SPF

In Your Domain's Control Panel or Zone Editor
* DKIM: Create a CNAME record for k1._domainkey.yourdomain.com with this value: dkim.mcsv.net.
* SPF: Create a TXT record for yourdomain.com with this value: v=spf1 include:servers.mcsv.net -all or add "include:servers.mcsv.net"  to your SPF record 

Note
* The URLs above are examples only. Replace "yourdomain.com" with the domain you want to authenticate.
