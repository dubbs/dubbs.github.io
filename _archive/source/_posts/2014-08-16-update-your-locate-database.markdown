---
layout: post
title: "Update your locate database"
date: 2014-08-16 16:15
updated: 2014-08-16 16:15
comments: true
categories: [unix, locate]
---

The `locate` command is great for searching the entire filesystem for files:

	locate my.cnf

Recently created files and directories might not show up, so update the index:

	locate updatedb
	/usr/libexec/locate.updatedb