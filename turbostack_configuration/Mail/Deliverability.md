---
order: 100
icon: mail
---
# Email Delivery Best Practices: Keep Your Messages Out of Spam
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

v=spf1 include:mail.example.com ip4:64.186.18.168 -all

- `v=spf1` indicates the version.
- `include:` lists authorized servers.
- `ip4:` lists authorized servers, but based on IPv4 address.
- `-all` specifies that any non-listed server should fail the SPF check.
3. **Test Your SPF Setup:** Tools like MXToolbox can validate your SPF record and ensure it’s correctly configured.

### 2. DKIM (DomainKeys Identified Mail)
DKIM adds a digital signature to your emails, allowing the recipient’s server to verify that the message hasn’t been altered in transit and that it genuinely came from your domain.

- **How it works:** The sending server attaches an encrypted signature to the email’s header. The recipient’s server retrieves the public key from your DNS records to verify the signature’s authenticity.

Activating DKIM on Turbostack is easily done via the `<a href="https://my.turbostack.app" target="_blank">`TurboStack App `</a>`! Simply navigate to your host and go to the 'Advanced' tab. Follow the instructions under 'Mail Settings' to set up DKIM.

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

Enforce SPF compliance with the `aspf` tag:
- Force strict compliance with `aspf=s` (RECOMMENDED)
- Set up relaxed compliance with `aspf=r` (*)

(*) In relaxed SPF Alignment, the MailFROM domain and the Header From domain must be an exact match or a parent/child match (i.e. example.com and child.example.com). The parent/child match type allows any subdomain and parent domain pair to generate a PASS result. Also worth noting, in the parent/child match scenario either the MailFROM domain or the Header From domain can be the parent or the child domain.

2. **Create a DMARC Record:** Add a TXT record to your DNS. Example:
_dmarc.example.com IN TXT "v=DMARC1; p=reject; aspf=s; rua=mailto:dmarc-reports@example.com;"

This record will strictly reject mails that do NOT originate from an SMTP server included in the origin domain's SPF record, and send a report to dmarc-reports@example.com.

### 4. Further optimisation
Properly configuring your DNS with SPF, DKIM, and DMARC is a significant step toward improving your email deliverability. However, if you’re still encountering issues, additional factors might be at play. Problems could stem from the content and formatting of your HTML email, SMTP server configurations, or even being blacklisted for other reasons.

To assess your current email deliverability, we recommend running a test on `<a href="https://mail-tester.com" target="_blank">`Mail Tester`</a>`. If your score isn’t perfect, the test report will highlight areas for improvement.

Need help interpreting the results or taking the next steps? Feel free to reach out to us—we’re here to assist!