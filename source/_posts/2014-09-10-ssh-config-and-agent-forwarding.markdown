---
layout: post
title: "SSH Config and Agent Forwarding"
date: 2014-09-10 21:50:59 -0600
updated: 2014-09-10 21:50:59 -0600
comments: true
categories: [unix, ssh]
---

Quick guide to setting up ssh config and agent forwarding.

## 1. Setup remote server

Enable authorized keys on remote, `/etc/sshd_config`:

	AuthorizedKeysFile  .ssh/authorized_keys

## 2. Setup client keys

Generate an ssh key

	ssh-keygen -t rsa

Copy public key to remote server

	ssh-copy-id -i ~/.ssh/id_rsa.pub user@example.com

Test connection using private key 

	ssh -i ~/.ssh/id_rsa user@example.com date

## 3. Setup client config

This allows for separate ssh configuration per host:

	touch ~/.ssh/config
	chmod 600 ~/.ssh/config

Add the following to `~/.ssh/config`:

	Host remoteServer1
	HostName example.com
	User user
	PubkeyAuthentication yes
	IdentityFile ~/.ssh/id_rsa

Test connection:

	ssh -T remoteServer1

## 4. Setup agent forwarding

`ssh-agent` is a user daemon which holds unencrypted ssh keys in memory.  This saves you from having to supply your passphrase for each connection.

Update `~/.ssh/config`:

	Host remoteServer1
	...
	ForwardAgent yes

Verify ssh-agent is running:

	echo "$SSH_AUTH_SOCK"

Verify your public key available to ssh-agent:

	ssh-add -L

If not, add a specific identity:

	ssh-add ~/.ssh/id_project1

### Resources:

http://www.unixwiz.net/techtips/ssh-agent-forwarding.html  
https://developer.github.com/guides/using-ssh-agent-forwarding/  
http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/  
https://kimmo.suominen.com/docs/ssh/  
http://blogs.perl.org/users/smylers/2011/08/ssh-productivity-tips.html  

