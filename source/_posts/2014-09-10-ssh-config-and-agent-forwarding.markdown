---
layout: post
title: "SSH Config and Agent Forwarding"
date: 2014-09-10 21:50:59 -0600
updated: 2014-09-11 11:14:59 -0600
comments: true
categories: [unix, ssh]
---

Quick guide to setting up ssh config and agent forwarding.

## 1. Setup remote server

Enable authorized keys on remote, `/etc/sshd_config`:

	RSAAuthentication yes
	PubkeyAuthentication yes
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

Test the connection:

	ssh remoteServer1 date

## 4. Setup agent forwarding

`ssh-agent` is a user daemon which holds unencrypted ssh keys in memory.  Key 
challenges are sent from a remote machine, through any intermediary servers 
and back to your local machine.  This saves you from having to store your
private keys on remote servers.

Turn on agent forwarding for your host, `~/.ssh/config`:

	Host remoteServer1
	...
	ForwardAgent yes

Verify ssh-agent is running:

	echo "$SSH_AUTH_SOCK"

Verify you have an identity loaded:

	ssh-add -L

If not, add an identity:

	ssh-add ~/.ssh/id_rsa

Login, logout, login to remote, first login requires passphrase.
Subsequent logins do not:

	ssh remoteServer1
	exit
	ssh remoteServer1

### Resources:

http://www.unixwiz.net/techtips/ssh-agent-forwarding.html  
https://developer.github.com/guides/using-ssh-agent-forwarding/  
http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/  
https://kimmo.suominen.com/docs/ssh/  
http://blogs.perl.org/users/smylers/2011/08/ssh-productivity-tips.html  

