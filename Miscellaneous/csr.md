---
hidden: true
---
# Introduction

Instead of using Let's Encrypt to generate an SSL certificate, you can obtain one from another certificate authority. To do this, you will need to create a Certificate Signing Request (CSR). For security reasons, we recommend generating the CSR on the server itself, so the private key does not need to be transferred from another computer.

---

# Certificate with Multiple Subject Alternative Names (SAN)

To generate the private key and a CSR containing multiple subject alternative names, follow these steps:

## 1. Create the .san File

Example configuration file (`server.san`):

```ini
[ req ]
default_bits = 4096
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no

[ req_distinguished_name ]
countryName = BE
stateOrProvinceName = OVL
localityName = Gent
organizationName = Hosted Power
commonName = www.hosted-power.com

[ req_ext ]
subjectAltName = @alt_names

[alt_names]
DNS.1 = www.hosted-power.com
DNS.2 = hosted-power.com
DNS.3 = hosted-power.be
DNS.4 = www.hosted-power.nl
DNS.5 = mail.hosted-power.com
```

**Hint:** The common name (`commonName`) must match your domain, e.g., *www.example.com*. For wildcard certificates, use `*.example.com` as the common name.

## 2. Generate the CSR

Use the following command to generate both the CSR and the private key:

```bash
openssl req -sha256 -new -newkey rsa:4096 -nodes -keyout server.key -out server.csr -config server.san
```

In this case, **server.csr** is our signing request. Provide this to your preferred CA to generate the required files (public key, CA certificate, ...etc)

---

# Turbostack

_We recommend adding the private key and the certificates via the GUI, because of the formatting._

When opening your server in the TurboStack GUI:

- Click **Accounts**
- Click the **cog** of the account
- Click the **cog** of the applications

![Turbostack GUI Step 1](image/csr/csr_account_panel.png)

You can now configure your application:

- Click **Hostnames** and scroll down a bit
- Select the **custom** certificate type
- Enter the **private key**
- Enter the **fullchain certificate** in the right order:
  - Public Key  
  - CAA Certificate  
  - Intermediary certificates (usually 1 or 2)
- Click **Save**
- Click **Save & Publish**

**Hint:** If the publish fails, the intermediary certificates could be in the wrong order.

![Turbostack GUI Step 2](image/csr/csr_account_hostname_custom_certificate.png)