---
title: "Initial Setup"
summary: "Notes on setting up the blog"
date: 2022-11-28T19:53:20-08:00
draft: false
tags:
- Web Site
---

## Notes on setting up the blog

### Subdomain

Created the subdomain by going into *Subdomain Management* in the DirectAdmin control panel. Added the **blog** subdomain. 

### SSL Certificate

Created the SSL certificate by going into *SSL Certificates* in the DirectAdmin control panel. Selected *Get automatic certificate from ACME Provider* and chose **Let's Encrypt** as the ACME provider. Under *Certificate Entries* chose *blog.steveandmimi.com*, *mail.steveandmimi.com*, *steveandmimi.com*, and *www.steveandmimi.com*. Clicked *SAVE* and the certificate was generated and installed.

### GitHub Workflow

I set up a GitHub workflow to build and deploy the site whenever changes are pushed. I created an SSH key in Hostrocket's web control panel. After creating it I had to authorize it by clicking the checkbox next to the key name then selecting 'Authorize'. Looking back at the *Create SSH Key* dialog I see there's an "Authorize" checkbox. I should have checked that.

I created repository secrets for the host, port, user, path, and key. (The key is the private ssh key.)

One of the steps I took created something that expires December 31, 2023. Now I can't remember which thing that was.

This is the workflow file:

```yaml
name: Hugo
on: 
    push:
        branches:
            - main
jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
            - name: Git checkout
              uses: actions/checkout@v3
              
            - name: Set up Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                hugo-version: 'latest'
                extended: true
                
            - name: Build
              run: hugo --minify
              
            - name: rsync deployment
              uses: burnett01/rsync-deployments@5.2.1
              with:
                switches: -avzr --delete --exclude="" --include="" --filter=""
                path: public/
                remote_path: ${{ secrets.DEPLOY_PATH }}
                remote_host: ${{ secrets.DEPLOY_HOST }}
                remote_port: ${{ secrets.DEPLOY_PORT }}
                remote_user: ${{ secrets.DEPLOY_USER }}
                remote_key: ${{ secrets.DEPLOY_KEY }}
                
            - name: Display status from deploy
              run: echo "${{ steps.deploy.outputs.status }}"
```