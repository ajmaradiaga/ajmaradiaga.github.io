---
layout: post
title: Adding and trusting a Certificate Authority (CA) in Fedora
---

When running Citrix on Fedora, I was getting an error message similar to the following: "SSL Error 61: You have not chosen to trust 'Certificate Authority'". Basically, the CA (certificate authority) cert was missing in my operating system. To fix this, we need to do the following:

1. Find the CA's certificate online.
2. Ensure that the certificate is in PEM format (if not, convert it to PEM).
3. Move the PEM file to `/etc/pki/ca-trust/source/anchors`.
4. Update the CA trust in the operating system.

In the example below, I add and trust GoDaddy's root G2 certificate:
```bash
# Download GoDaddy - G2 root certificate
wget https://ssl-ccp.godaddy.com/repository/gdroot-g2.crt

# Convert CRT to PEM
openssl x509 -in gdroot-g2.crt -out gdroot-g2.pem -outform PEM

# Move the PEM cert and update the CA trust
sudo mv gdroot-g2.pem /etc/pki/ca-trust/source/anchors/
sudo update-ca-trust
```

### References:
- Convert CRT to PEM: https://stackoverflow.com/questions/4691699/how-to-convert-crt-to-pem
- Trusting additional CAs in Fedora: https://www.happyassassin.net/2015/01/14/trusting-additional-cas-in-fedora-rhel-centos-dont-append-to-etcpkitlscertsca-bundle-crt-or-etcpkitlscert-pem/
