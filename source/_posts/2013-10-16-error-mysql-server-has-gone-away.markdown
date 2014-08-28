---
layout: post
title: "Error: MySQL server has gone away"
date: 2013-10-16 13:17
comments: true
categories: [error, mysql]
---

When trying to create a new mysql user, I came across the error:

    ERROR 2006 (HY000) at line 1: MySQL server has gone away

The fix turned out to be simple, as the documentation states, "[mysql_upgrade](http://dev.mysql.com/doc/refman/5.0/en/mysql-upgrade.html) should be executed each time you upgrade MySQL".  The following command fixed the problem.

    mysql_upgrade --password
