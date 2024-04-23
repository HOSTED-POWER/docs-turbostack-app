---
order: 160
icon: lock
---

# SSL/TLS configuration

A Certificate authority is a trusted entity responsible for issuing digital certificates used to authenticate the identities of entities such as websites, servers, users, and devices on the internet or within a private network

## Let's Encrypt Certificate

LetsEncrypt is a certificate authority that provides X.509 certificates for Transport Layer Security (TLS) encryption at no charge. The certificate is valid for 90 days, during which renewal can take place at anytime. The offer is accompanied by an automated process designed to overcome manual creation, validation, signing, installation, and renewal of certificates for secure websites 

The key principles behind Let's Encrypt, taken from their [website](https://letsencrypt.org/){:target="_blank"} <a href="http://www.letsencrypt.org" target="_blank">website</a>
* Free - Anyone who owns a domain name can use Let’s Encrypt to obtain a trusted certificate at zero cost.
* Automatic - Software running on a web server can interact with Let’s Encrypt to painlessly obtain a certificate, securely configure it for use, and automatically take care of renewal.
* Secure - Let’s Encrypt will serve as a platform for advancing TLS security best practices, both on the CA side and by helping site operators properly secure their servers.
* Transparent - All certificates issued or revoked will be publicly recorded and available for anyone to inspect.
* Open: The automatic issuance and renewal protocol is published as an open standard that others can adopt.
* Cooperative: Much like the underlying Internet protocols themselves, Let’s Encrypt is a joint effort to benefit the community, beyond the control of any one organization.

## Third Party Certificate

### Comodo

[Contact our support](../support/standard_support) if you need to install a third party certificate.
