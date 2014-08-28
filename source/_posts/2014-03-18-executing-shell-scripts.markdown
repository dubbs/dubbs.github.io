---
layout: post
title: "Executing Shell Scripts"
date: 2014-03-18 23:53
comments: true
categories: [unix, mac]
---

Here are some of the basic ways you can execute scripts on the command line.

`source` or `.` will read and execute commands from filename in the **current shell** environment.  
Any environment variables set within the script will remain after exit.

	. test.sh
	source test.sh

`sh` or `bash` will fork a **new shell** with the specified interpreter.

	sh test.sh
	bash test.sh

`./` will also fork a **new shell**, but the file needs to be set as executable and interpreter will be derived from the shebang (`#!/bin/sh`).

	chmod +x test.sh
	./test.sh