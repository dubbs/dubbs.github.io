---
layout: post
title: "Shell Modes and Init Files"
date: 2014-03-19 23:20
updated: 2014-10-10 17:52
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

You will be forwarded to the users home directory, with the users
environment.

- log in to a remote system via SSH
- new terminal tab, Mac OS X
- `sudo su -`

files sourced:

	# The systemwide initialization file
	/etc/profile

	# The personal initialization files, first one found, in order
	~/.bash_profile
	~/.bash_login
	~/.profile

#### Non-login + Interactive

You will stay in the current directory, but will have the users 
environment.

- new terminal tab, linux
- start new shell process ($ bash)
- execute script remotely and request terminal (ssh user@host -t 'echo $PWD')
- `sudo su`

files sourced:

	# The individual per-interactive-shell startup file
	~/.bashrc

#### Non-login + Non-Interactive

You will stay in the current directory and keep your current
environment.

- run an executable with #!/usr/bin/env bash shebang
- run a script ($ bash test.sh)
- execute script remotely (ssh user@host 'echo $PWD')

files sourced:

	source $BASH_ENV


References:

- [Linux Command](http://linuxcommand.org/)
- [Unix Shell Initialization](https://github.com/sstephenson/rbenv/wiki/Unix-shell-initialization)
- [Switch User] (http://askubuntu.com/questions/376199/sudo-su-vs-sudo-i-vs-sudo-bin-bash-when-does-it-matter-which-is-used)

