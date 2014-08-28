---
layout: post
title: "Joining files on the command line"
date: 2013-10-15 16:00
comments: true
categories: unix
---

Here's an extremely easy way to join files in unix.  It uses the `()` subshell operator, which waits for the enclosed command to return before passing the result to standard out.

	(cat file1 file2)> file3
