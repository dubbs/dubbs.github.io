---
layout: post
title: "SSL Certificate for Localhost"
date: 2014-09-24 22:22:09 -0600
comments: true
categories: [ssl, apache]
---

Create a self-signed certificate and move it to the apache conf
directory.

```sh
# generate private key
openssl genrsa -des3 -out server.key 1024

# generate csr
openssl req -new -key server.key -out server.csr

# remove passphrase from private key
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key

# create self-signed cert 
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

# move to apache conf dir
cp server.crt /etc/apache2/server.crt
cp server.key /etc/apache2/server.key
```

Enable the default ssl configuration, `/etc/apache2/httpd.conf`

```sh
# uncomment the following line
Include /private/etc/apache2/extra/httpd-ssl.conf
```

Add host entry for development site, `/etc/hosts`

```sh
127.0.0.1 devsite.local
```

Modify the default virtual host entry, `/etc/apache2/extra/httpd-ssl.conf`

```sh
# update config for site
DocumentRoot "/Library/WebServer/Documents/devsite"
ServerName devsite.local:433
```

Restart Apache

```sh
sudo apachectl restart
```
