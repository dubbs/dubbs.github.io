---
layout: post
title: "Deleting MySQL bin files"
date: 2014-08-18 15:25
updated: 2014-08-18 15:25
comments: true
categories: [unix, mysql]
---

To view your current bin files:

	$ mysql -u root -p
	mysql> SHOW MASTER LOGS;

To clear all logs but the last one:

	mysql> PURGE MASTER LOGS TO 'mysql-bin.000107';

Open `my.cnf` and comment out the following lines to prevent logging in the future:

	# log-bin=mysql-bin

Restart your server and confirm logging is now disabled:

	$ mysql.server restart
	$ mysql -u root -p
	mysql> SHOW MASTER LOGS;
