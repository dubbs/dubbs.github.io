---
layout: post
title: "Querying MySQL Configuration"
date: 2013-09-23 12:40
comments: true
categories: mysql
---
 
MySQL system variables can be queried using the [show variables](http://dev.mysql.com/doc/refman/5.7/en/show-variables.html) syntax:
 
	mysql -uUSERNAME -pPASSWORD -e 'SHOW VARIABLES LIKE "ft_min_word_len"'

It's also useful to find out with `my.cnf` file MySQL will try to load on init:
 
	mysql --help | grep cnf