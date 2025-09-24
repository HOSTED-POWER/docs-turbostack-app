---
order: 100
icon: paper-airplane
---
# Email Deliverability
Ensuring your emails land in the recipient’s inbox—and not their spam folder—requires careful attention to email delivery practices. Poor email delivery can harm your reputation, reduce engagement, and hinder communication with your audience.

This article explores best practices to improve email deliverability and provides an in-depth explanation of key email authentication protocols like SPF, DKIM, and DMARC. Implementing these strategies will help you avoid the spam folder and maintain a strong sender reputation.

## Understanding Email Authentication Protocols

### 1. SPF (Sender Policy Framework)
SPF is an email authentication protocol designed to prevent spoofing by specifying which mail servers are authorized to send emails on behalf of your domain. When you set up an SPF record, it tells recipient mail servers, “Here’s the list of IP addresses or servers allowed to send emails for this domain.”

- **How it works:** When an email is received, the recipient’s server checks the sending IP against the domain’s SPF record. If the IP isn’t listed, the email is more likely to be marked as spam or rejected.
- **Best practice:** Always keep your SPF record updated to include all servers or services you use to send emails (e.g., your hosting provider, CRM, or email marketing tool).

#### How to Implement SPF
1. **Define Your Sending Sources:** Identify all the mail servers and third-party services you use to send emails, such as your website hosting, CRM, or marketing platforms.
2. **Create an SPF Record:** Use your DNS manager to add a TXT record for your domain. An example SPF record might look like this:

`v=spf1 a mx include:mail.example.com ip4:64.186.18.168 -all`

- `v=spf1` indicates the version.
- `a` includes the hostname's A record(s) in the SPF lookup.
- `mx` includes the hostname's MX record(s) in the SPF lookup.
- `include:` lists authorized servers.
- `ip4:` lists authorized servers, but based on IPv4 address.
- `ip6:` lists authorized servers, but based on IPv6 address.
- `-all` specifies that any non-listed server should fail the SPF check.

!!! Important
SPF records are limited to 10 DNS lookups per authentication check! Exceeding the 10-lookup limit results in a permanent error, causing SPF verification to fail.

To stay within this limit, we advise the following:

- Minimize include mechanisms by consolidating authorized senders.
- Avoid unnecessary use of a and mx lookups.
- Replace mechanisms with static IP ranges when feasible.
- Use SPF record flattening tools to generate a single, simplified record.
!!!

3. **Test Your SPF Setup:** Tools like MXToolbox can validate your SPF record and ensure it’s correctly configured.

### 2. DKIM (DomainKeys Identified Mail)
DKIM adds a digital signature to your emails, allowing the recipient’s server to verify that the message hasn’t been altered in transit and that it genuinely came from your domain.

- **How it works:** The sending server attaches an encrypted signature to the email’s header. The recipient’s server retrieves the public key from your DNS records to verify the signature’s authenticity. Example:

`cloud._domainkey.example.com IN TXT "k=rsa; t=s; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDDmzRmJRQxLEuyYiyMg4suA2SyMwR5MGHpP9diNT1hRiwUd/mZp1ro7kIDTKS8ttkI6z6eTRW9e9dDOxzSxNuXmume60Cjbu08gOyhPG3GfWdg7QkdN6kR4V75MFlw624VY35DaXBvnlTJTgRg/EW72O1DiYVThkyCgpSYS8nmEQIDAQAB"`

!!! Info
Activating DKIM on Turbostack is easily done via the [TurboStack App](https://my.turbostack.app "TurboStack App")! Simply navigate to your host and go to the 'Advanced' tab. Follow the instructions under 'Mail Settings' to set up DKIM.
!!!

### 3. DMARC (Domain-based Message Authentication, Reporting, and Conformance)
DMARC ties SPF and DKIM together to provide a comprehensive email authentication framework. It tells recipient servers how to handle emails that fail SPF or DKIM checks and enables you to receive reports on failed authentication attempts.

- **How it works:** DMARC policies are defined in your DNS records, specifying whether to quarantine, reject, or do nothing with emails that fail authentication.

#### How to Implement DMARC
1. **Draft a DMARC Policy:** 

Define how to handle authentication failures using the `p` tag:
- `p=none`: Monitor only (no enforcement).
- `p=quarantine`: Mark failing emails as spam.
- `p=reject`: Reject failing emails outright. (RECOMMENDED)

Set up e-mail reporting using the `rua` tag: 
- e.g. `rua=mailto:dmarc-reports@example.com`
- The `ruf` tag can be used to send forensic reports.
- This setting is entirely optional! You can omit the tag altogether.

Enforce SPF compliance with the `aspf` tag:
- Force strict compliance with `aspf=s` (RECOMMENDED)
- Set up relaxed compliance with `aspf=r` (*)

(*) In relaxed SPF Alignment, the MailFROM domain and the Header From domain must be an exact match or a parent/child match (i.e. example.com and child.example.com). The parent/child match type allows any subdomain and parent domain pair to generate a PASS result. Also worth noting, in the parent/child match scenario either the MailFROM domain or the Header From domain can be the parent or the child domain.

2. **Create a DMARC Record:** Add a TXT record to your DNS. Example:
`_dmarc.example.com IN TXT "v=DMARC1; p=reject; aspf=s; rua=mailto:dmarc-reports@example.com;`

This record will strictly reject mails that do NOT originate from an SMTP server included in the origin domain's SPF record, and send a report to dmarc-reports@example.com.

### 4. Further optimisation
Properly configuring your DNS with SPF, DKIM, and DMARC is a significant step toward improving your email deliverability. However, if you’re still encountering issues, additional factors might be at play. Problems could stem from the content and formatting of your HTML email, SMTP server configurations, or even being blacklisted for other reasons.

To assess your current email deliverability, we recommend running a test on [Mail Tester](https://mail-tester.com "Mail Tester"). If your score isn’t perfect, the test report will highlight areas for improvement.

Need help interpreting the results or taking the next steps? Feel free to reach out to us—we’re here to assist!

### 5. Blocklists (RBLs) and How They Affect Deliverability  

Even with SPF, DKIM, and DMARC correctly set up, your emails may still struggle to reach inboxes if your server’s IP address or domain has been placed on a **blocklist** (often called an **RBL – Realtime Block List**).  

#### What is a Blocklist / RBL?  
A blocklist is a database of IP addresses or domains flagged for sending spam or unwanted email. Many email providers and spam filters query these RBLs to decide whether to deliver, filter, or reject incoming messages.  

If your server is listed, your emails may:  
- Go straight to spam.  
- Be delayed.  
- Be rejected entirely.  

**Info:** Different mail services rely on different RBLs for spam filtering! Because of this, your email may reach some recipients while being blocked by others. Which RBL is consulted depends entirely on the recipient’s mail provider.

#### Why You Might Be Blocklisted  
Before looking at blocklist issues, make sure your email authentication and deliverability are set up correctly as described earlier (SPF, DKIM, DMARC, and content best practices). These are the **first priority**.  

If problems persist after that, common reasons for blocklisting include:  
- Your server was used to send spam (e.g., due to a hacked website or weak email account password).  
- You’re sending to invalid or outdated email addresses.  
- Too many recipients marked your emails as spam.  
- Bulk email (such as newsletters) is being sent directly from your webserver instead of a proper mail delivery service.  

⚠️ **Important:** Bulk mailing should **never** be done from your local SMTP service. Sending newsletters or large campaigns this way will almost always lead to blacklisting. Instead, always use a professional service such as [SendGrid](https://sendgrid.com/), [Amazon SES](https://aws.amazon.com/ses/), or [Mailchimp](https://mailchimp.com/) for mass mailings.  

#### How to Check if You’re Listed  
You can quickly check if your domain or server IP is on a blocklist using these tools:  
- [MXToolbox Blacklist Check](https://mxtoolbox.com/blacklists.aspx)  
- [Spamhaus Blocklist Removal Center](https://check.spamhaus.org/)  
- [MultiRBL Lookup](https://multirbl.valli.org/)  

Simply enter your server’s IP address to see if it appears on any RBLs.  

**Tip:** If your [Mail Tester](https://www.mail-tester.com/) score looks fine but your messages are still not being delivered, it’s worth checking whether your IP or domain is listed on an RBL.  

#### What to Do if You’re Blocklisted  
1. **Secure your server**  
   - Scan for hacked sites, malware, or compromised accounts.  
   - Change email account passwords if needed.  

2. **Avoid bulk mailing locally**  
   - Do not send newsletters or campaigns from your webserver.  
   - Move all bulk mailing to a dedicated provider like SendGrid.  

3. **Request delisting**  
   - Each RBL has a removal process (usually an online form).  
   - Only request delisting once you’re sure the problem is fixed, otherwise you risk being re-listed.  

---

✅ **Tip:** After resolving the issue, monitor your IP reputation regularly. Staying off RBLs requires both secure server practices and responsible mailing practices.