---
layout: post
title: "Using the Loopback Interface"
date: 2014-03-08 15:14
comments: true
categories: mac
---

The hosts file is used to map hostnames to ip addresses.

**localhost** is the hostname for the loopback network interface, in `/etc/hosts`:

	# IPv4 loopback address
	127.0.0.1 localhost

	# IPv6 loopback address
	::1 localhost

	# IPv6 link-local address
	fe80::1%lo0 localhost

An **loopback** address is used to send a packet to itself, whereas **link-local** 
allows packet transfer between devices on the local link (not routable).

Loopback addresses are often used in web development:

	127.0.0.1 localhost project1.local project2.local

They are also used to increase privacy and security.  Here is a very helpful [resource](http://someonewhocares.org/hosts/hosts) on the subject.

After updating your hosts file, be sure to clear the directory service cache:

	dscacheutil -flushcache
