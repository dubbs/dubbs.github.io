---
layout: post
title: "Shell Modes and Init Files"
date: 2014-03-19 23:20
updated: 2014-03-19 23:20
comments: true
categories: [unix, mac]
---

## Modes

There are two main shell modes:

### 1. Login

When a user logs in with a non-graphical interface or SSH.

### 2. Interactive

When a user has a prompt and standard in/out are connected to the terminal.

## Combinations of Modes

A shell can be initialized with the following mode combinations:

#### Login + Interactive

- log in to a remote system via SSH
- new terminal tab, Mac OS X

files sourced:

	# The systemwide initialization file
	/etc/profile

	# The personal initialization files, first one found, in order
	~/.bash_profile
	~/.bash_login
	~/.profile

#### Non-login + Interactive

- new terminal tab, linux
- start new shell process ($ bash)
- execute script remotely and request terminal (ssh user@host -t 'echo $PWD')

files sourced:

	# The individual per-interactive-shell startup file
	~/.bashrc

#### Non-login + Non-Interactive

- run an executable with #!/usr/bin/env bash shebang
- run a script ($ bash test.sh)
- execute script remotely (ssh user@host 'echo $PWD')

files sourced:

	source $BASH_ENV


References:

- [Linux Command](http://linuxcommand.org/)
- [Unix Shell Initialization](https://github.com/sstephenson/rbenv/wiki/Unix-shell-initialization)

