---
hidden: true
---
# A security audit revealed an OpenSSH vulnerability
When running a security audit, you may see alerts about OpenSSH vulnerabilities on your server. These often appear because an older version of OpenSSH is reported, and scanners assume it is vulnerable.  
In most cases, this is a **false positive**. 

Distributions like Debian regularly backport security patches while keeping the same upstream version number. This means your server may be secure, even if the version number looks outdated.

Our automation checks for updates, multiple times a day to keep your environments protected.

## Why This Happens
- **Backported fixes**: Distributions apply security patches to older versions without incrementing the version number.  
- **Scanner limitations**: Many scanners only check version numbers against CVE databases and don’t account for backported patches.  

As a result, an audit might flag `OpenSSH_8.0p1` as vulnerable, even though the specific CVE has already been patched.

## Verifying Security Status
Debian maintains a [Security Tracker](https://security-tracker.debian.org/tracker/) where you can check whether a CVE is patched. For example:  
[CVE-2024-6387](https://security-tracker.debian.org/tracker/CVE-2024-6387)

To check your installed version:

```bash
# Show installed OpenSSH packages:
dpkg -l | grep openssh

# Show running OpenSSH version:
ssh -V
```
