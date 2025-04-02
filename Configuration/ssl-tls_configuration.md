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