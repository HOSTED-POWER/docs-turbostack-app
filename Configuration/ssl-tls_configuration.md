---
order: 160
icon: lock
---

# SSL Certificates

An SSL certificate is a digital certificate that encrypts data between a website and its visitors, ensuring secure communication. It verifies a website’s identity and enables HTTPS, protecting sensitive information like passwords and credit card details from hackers. SSL is essential for the correct functioning of your website over HTTPS! 

## Certificate Authorities (CA)

A Certificate authority is a trusted entity responsible for issuing digital certificates used to authenticate the identities of entities such as websites, servers, users, and devices on the internet or within a private network.

### Let's Encrypt

LetsEncrypt is a certificate authority that provides X.509 certificates for Transport Layer Security (TLS) encryption at no charge. The certificate is valid for 90 days, during which renewal can take place at any time. The offer is accompanied by an automated process designed to overcome manual creation, validation, signing, installation, and renewal of certificates for secure websites.

Installing Let’s Encrypt on your TurboStack server is quick and simple. The only requirement is that your hostname(s) correctly point to the server in DNS. For detailed setup instructions, click [here](https://docs.turbostack.app/turbostack-app/howto_newuser/ "here").

The key principles behind Let's Encrypt, taken from their <a href="http://www.letsencrypt.org" target="_blank">website</a>
* Free - Anyone who owns a domain name can use Let’s Encrypt to obtain a trusted certificate at zero cost.
* Automatic - Software running on a web server can interact with Let’s Encrypt to painlessly obtain a certificate, securely configure it for use, and automatically take care of renewal.
* Secure - Let’s Encrypt will serve as a platform for advancing TLS security best practices, both on the CA side and by helping site operators properly secure their servers.
* Transparent - All certificates issued or revoked will be publicly recorded and available for anyone to inspect.
* Open: The automatic issuance and renewal protocol is published as an open standard that others can adopt.
* Cooperative: Much like the underlying Internet protocols themselves, Let’s Encrypt is a joint effort to benefit the community, beyond the control of any one organization.

### Third Party Certificates

Need an SSL certificate other than Let’s Encrypt? No problem! We offer Sectigo certificates—simply contact support for more details.

If you’ve purchased a certificate from another provider, you can still install it easily on your TurboStack server through the Account Management section of the [TurboStack App](https://my.turbostack.app "TurboStack App"). More info on how to do so can be found [here](https://docs.turbostack.app/turbostack-app/howto_newuser/ "here")

### Order a standalone SSL certificate

> **It is almost never required to purchase a standalone SSL certificate on TurboStack! Let's Encrypt will cover almost all cases. If you're unsure if this is correct for you, check with our support team first.**

Ordering a standalone certificate can be done easily through your [customer portal]().

Unless you're looking for a specific kind of certificate, or were instructed to get a specific kind of certificate, you're looking for a 'Sectigo Positive SSL' certificate. When ordering, you'll be asked for a CSR (Certificate Signing Request). Either you know what this is already, you've been given one to use for this order, or you need to choose the option 'I don't have my CSR ready. I want to generate one now.'.

Once you do, you'll be asked to fill in the organization data of the organization requesting this certificate. Specifically for this type of certificate (Sectigo Positive), this data will NOT be verified against public records. When asked for the 'Certificate Common Name', you need to provide the domain you're looking to use this certificate for.

Lastly, you need to choose an email address for the email validation of your certificate to be sent to. This MUST be an email address on the root domain of the requested domain, and can only be one of 5 specific users:

> admin@domain.com
> administrator@domain.com
> hostmaster@domain.com
> webmaster@domain.com
> postmaster@domain.com

Once you've completed your order, you'll be sent an email to that chosen address, with instructions to validate your request. Once validated, within 10 minutes your certificate should be issued, and available in your customer portal!
