---
layout: post
title: "Error: Failed to Create Host-Only Adapter"
date: 2014-03-07 20:50
comments: true
categories: [error, vagrant]
---

When trying to run `vagrant up`, received error:

	There was an error while executing `VBoxManage`, a CLI used by Vagrant
	for controlling VirtualBox. The command and stderr is shown below.

	Command: ["hostonlyif", "create"]

	Stderr: 0%...
	Progress state: NS_ERROR_FAILURE
	VBoxManage: error: Failed to create the host-only adapter
	VBoxManage: error: VBoxNetAdpCtl: Error while adding new interface: failed to open /dev/vboxnetctl: No such file or directory

	VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component HostNetworkInterface, interface IHostNetworkInterface
	VBoxManage: error: Context: "int handleCreate(HandlerArg*, int, int*)" at line 68 of file VBoxManageHostonly.cpp


This happens because another VM is running in VirtualBox, run:

	sudo /Library/StartupItems/VirtualBox/VirtualBox restart

If VirtualBox is loading on startup, remove it:

	cd /Library/StartupItems
	rm -r VirtualBox
