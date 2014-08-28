---
layout: post
title: "Mounting an AFP Filesystem"
date: 2013-09-23 13:35
comments: true
categories: [unix, mac]
---

Here's a simple way to connect to an Apple Filing Protocol (AFP) based server and access shared files.

	mkdir /Volumes/my-share
	mount_afp "afp://192.168.0.1/folder/" /Volumes/my-share

You can access your new connection in either `/Volumes` or in left pane of the finder.
