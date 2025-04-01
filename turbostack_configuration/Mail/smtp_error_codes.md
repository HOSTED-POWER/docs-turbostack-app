---
order: 50
icon: mail
---

# SMTP Errors

## Introduction ##

Freemail providers such as Gmail and Hotmail provide specific error codes that indicate issues with email delivery. In this article, we will learn what they mean and how we can resolve these issues.

If you've received an email from our support team with a link to this article, it means we detected an alert associated with one of the error codes listed below. This guide will help you identify the issue and find steps to resolve it.

Many of these errors stem from email deliverability issues. To improve your email deliverability, visit [this page](https://docs.turbostack.app/turbostack_configuration/mail/deliverability/ "this page").

If you have any questions or difficulties, don't hesitate to contact us!

## Error codes ##

**550-5.7.1 (Generic)**

- ** Meaning: This is a general status code indicating an issue with the sender’s reputation, resulting in emails being blocked by the receiving mail server. It is used by many mail providers, who don’t provide more specific reasons for blocking mails.
- ** Solution: As the exact cause of distrust is unclear from the error message, please check all other 550-5.7.1 codes. 

**550-5.7.1 "This message is likely unsolicited email"**

- ** Meaning: To reduce spam, the receiving mail server has blocked the message because it is likely unsolicited email.
- ** Solution: Review email content and ensure compliance with the receiving mail server’s anti-spam policies. Refer to their guidelines for more information. 

**550-5.7.1 "This message is likely suspicious due to the very low reputation of the sending IP address"**

- Meaning: The receiving mail server blocked the message because the sending IP address has a very low reputation.
- Solution: Improve the reputation of the sending IP by sending compliant, non-spammy emails and adhering to the receiver's email sender guidelines. 

**550-5.7.1 "SPF check failed"**

- Meaning: The email’s SPF record did not authorize the sending IP.
- Solution: Verify that the sending IP is included in the domain’s SPF record. 

**550-5.7.1 "DKIM signature verification failed"**

- Meaning: The DKIM signature in the email did not match the public key in DNS.
- olution: Ensure that the correct private key is used to sign emails and the DNS entry is properly configured. 

**550-5.7.24 "The SPF record of the sending domain has one or more suspicious entries"**

- Meaning: The SPF record includes entries that are potentially insecure.
- Solution: Review and remove unnecessary or insecure entries from the SPF record. 

**550-5.7.25 "The IP address sending this message does not have a PTR record setup"**

- Meaning: The sending IP lacks a reverse DNS (PTR) record or the PTR record does not match the sending IP.
- Solution: Ensure that a valid PTR record is set up for the sending IP and that forward and reverse DNS entries match. 

**550-5.7.25 "The sending IP does not match the IP address of the hostname specified in the pointer (PTR) record"**

- Meaning: There is a mismatch between the sending IP and the PTR record.
- Solution: Correct the PTR record or ensure the sending IP aligns with it. 

**550-5.7.26 "This email has been blocked because the sender is unauthenticated"**

- Meaning: The email lacks proper authentication via SPF or DKIM.
- Solution: Authenticate with either SPF or DKIM and review setup instructions. 

**550-5.7.26 "The MAIL FROM domain has an SPF record with a hard fail policy (-all) but it fails to pass SPF checks"**

- Meaning: The SPF record specifies a hard fail, but the email failed SPF authentication.
- Solution: Ensure that the sending IP is authorized in the SPF record. 

**550-5.7.26 "Unauthenticated email from [domain-name] is not accepted due to domain's DMARC policy"**

- Meaning: The email failed DMARC authentication.
- Solution: Align SPF and DKIM with the DMARC policy. 

**550-5.7.27 "This email has been blocked because SPF does not pass"**

- Meaning: Gmail requires SPF authentication for large senders, but SPF failed.
- Solution: Configure SPF correctly to include all sending IPs. 

**550-5.7.28 "There is an unusual rate of unsolicited email originating from your IP address"**

- Meaning: The IP is sending an abnormally high rate of unsolicited emails.
- Solution: Reduce email sending volume and adhere to the receiving mail server’s bulk sender guidelines. 

**550-5.7.30 "This email has been blocked because DKIM does not pass"**

- Meaning: DKIM authentication failed.
- Solution: Set up DKIM correctly and ensure outgoing emails are signed with the proper private key.

