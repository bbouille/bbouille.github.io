---
title: Set dedicated TLS certificate for Nessus server 
layout: post
permalink: /2016/02/23/letsencrypt-for-nessus/
tags: [certificate, letsencrypt, nessus]
---

Environment :
- Debian 7.x (x64) via [OVH Virtual Private Server (VPS)](https://www.ovh.com)
- [Nessus Home (v6.5.5)](https://www.tenable.com/products/nessus-home)
- [Certificate provider](https://letsencrypt.org/certificates)

The installation of LetsEncrypt tool is incredibly fast with git ! The certificate generation is really simple and the deployment in nessus application is straightforward.

### Get LetsEncrypt tool

From your home directory :

```bash
cd ~
git clone https://github.com/letsencrypt/letsencrypt
```

### Generate the certificate

Stop the nessus deamon :

```bash
/etc/init.d/nessusd stop
```

Generate a new certificate with LetsEncrypt assistant :

```bash
cd ~/letsencrypt
./letsencrypt-auto --agree-dev-preview --server https://acme-v01.api.letsencrypt.org/directory auth
```

### Deploy the certificate

Copy the following files with root priviledges :

```bash
sudo cp -i /etc/letsencrypt/live/scan.bbouille.eu/fullchain.pem /opt/nessus/com/nessus/CA/servercert.pem
sudo cp -i /etc/letsencrypt/live/scan.bbouille.eu/privkey.pem /opt/nessus/var/nessus/CA/serverkey.pem
sudo cp -i /etc/letsencrypt/live/scan.bbouille.eu/chain.pem /opt/nessus/com/nessus/CA/cacert.pem
```

Then restart the nessus daemon :

```bash
/etc/init.d/nessusd start
```

Check the result :

![Cert deployed](/images/scan-cert.png)

Reference :
[1]: http://static.tenable.com/documentation/nessus_6.4_installation_guide.pdf
[2]: http://jerrygamblin.com/2015/11/04/letsencrypt-org-tls-certificate-with-nessus/

