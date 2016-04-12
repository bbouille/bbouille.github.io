---
title: Set dedicated TLS certificate for Nessus server 
layout: post
permalink: /2016/02/23/letsencrypt-for-nessus/
tags: [certificate, letsencrypt, nessus]
---

The installation of LetsEncrypt tool is incredibly fast with git ! The certificate generation is really simple and the deployment in nessus application is straightforward. 
Notes inspired by this [post](http://jerrygamblin.com/2015/11/04/letsencrypt-org-tls-certificate-with-nessus/).

## Environment

  * OVH Virtual Private Server [(VPS)](https://www.ovh.com) with Debian 7.x (x64) OS
  * [Nessus Home (v6.5.5)](https://www.tenable.com/products/nessus/select-your-operating-system) : download the "Nessus Home" version for Debian 6 and 7 / Kali Linux 1 AMD64
  * [letsencrypt.org](https://letsencrypt.org/certificates) : a free certificate provider without registration

## Get LetsEncrypt tool

On the OVH server, clone the `letsencrypt` repository in your home directory:

```bash
cd ~
git clone https://github.com/letsencrypt/letsencrypt
```

## Generate the certificate

Stop the nessus deamon :

```bash
/etc/init.d/nessusd stop
```

Generate a new certificate with LetsEncrypt assistant:

```bash
cd ~/letsencrypt
./letsencrypt-auto --agree-dev-preview --server https://acme-v01.api.letsencrypt.org/directory auth
```

## Deploy the certificate

Copy the following files with root priviledges using `sudo` :

```bash
sudo cp -i /etc/letsencrypt/live/scan.bbouille.eu/fullchain.pem /opt/nessus/com/nessus/CA/servercert.pem
sudo cp -i /etc/letsencrypt/live/scan.bbouille.eu/privkey.pem /opt/nessus/var/nessus/CA/serverkey.pem
sudo cp -i /etc/letsencrypt/live/scan.bbouille.eu/chain.pem /opt/nessus/com/nessus/CA/cacert.pem
```

Then restart the nessus daemon :

```bash
/etc/init.d/nessusd start
```

## Result

Connect to the nessus web application and check the certificate :
![Cert deployed](/images/scan-cert.png)

## Limitation

Please note that your certificate has a short life span : Let’s Encrypt CA issues short-lived certificates (90 days). See the documentation to renew the certificate : [https://letsencrypt.readthedocs.org/en/latest/using.html#renewal](https://letsencrypt.readthedocs.org/en/latest/using.html#renewal)
