---
layout: post
title: "Setup PHP with Apache and on the CLI"
date: 2015-02-14 11:59:16 -0600
updated: 2015-02-14 11:59:16 -0600
comments: true
categories: [apache, php, cli]
---

Install PHP using brew:

``` bash
brew update
brew search php5
brew install php56
```

Locate `libphp.so`

``` bash
sudo /usr/libexec/locate.updatedb
locate libphp5.so
# /usr/local/Cellar/php56/5.6.5/libexec/apache2/libphp5.so
```

Tell Apache which PHP to use this shared object `.so`:

``` bash
httpd -V | grep ROOT
# -D HTTPD_ROOT="/usr"
vi /etc/apache2/httpd.conf
# this following path is relative to /usr
LoadModule php5_module local/Cellar/php56/5.6.5/libexec/apache2/libphp5.so
```

Add PHP to your path:

``` bash
export PATH="$(brew --prefix homebrew/php/php56)/bin:$PATH"
php -v
```
