---
layout: post
title: "Verifying Data using Checksums"
date: 2014-03-07 16:31
updated: 2014-03-07 16:31
comments: true
categories: mac
---

Checksums are used verify data integrity and in some cases, authenticity.

It is especially important to run checksums on large files, such as operating systems, where 100% completeness is desired.  As file size increases, so will the possibility of errors during transmission.

To check a single file on Mac OS X, simply run:

	shasum -a 256 file.iso

To check multiple files using a SHA256SUMS file:

	cd ~/Downloads
	shasum -a 256 -c 1.4.3_SHA256SUMS 2>&1 | grep OK

If the file was verified you should see:

	Vagrant-1.4.3.dmg: OK

[HowToSHA256SUM](https://help.ubuntu.com/community/HowToSHA256SUM)
