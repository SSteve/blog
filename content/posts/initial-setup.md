---
title: "Initial Setup"
date: 2022-11-28T19:53:20-08:00
draft: false
---

## Notes on setting up the blog

### Subdomain

Created the subdomain by going into *Subdomain Management* in the DirectAdmin control panel. Added the **blog** subdomain. 

### SSL Certificate

Created the SSL certificate by going into *SSL Certificates* in the DirectAdmin control panel. Selected *Get automatic certificate from ACME Provider* and chose **Let's Encrypt** as the ACME provider. Under *Certificate Entries* chose *blog.steveandmimi.com*, *mail.steveandmimi.com*, *steveandmimi.com*, and *www.steveandmimi.com*. Clicked *SAVE* and the certificate was generated and installed.