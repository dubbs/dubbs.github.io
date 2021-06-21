---
layout: post
title: "Setup MySQL with a Launch Agent"
date: 2015-02-14 12:25:36 -0600
updated: 2015-02-14 12:25:36 -0600
comments: true
categories: [mysql]
---

Install MySQL using brew:

``` bash
brew update
brew search mysql
brew install mysql
```

Add a launch agent for MySQL, which runs on behalf of the current user:

``` bash
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
```

Start the launch agent now:

``` bash
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```
