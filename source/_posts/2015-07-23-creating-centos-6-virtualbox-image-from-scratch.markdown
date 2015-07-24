---
layout: post
title: "Creating a CentOS 6.6 VirtualBox Image from Scratch"
date: 2015-07-23 11:40:14 -0600
modified: 2015-07-23 11:40:14 -0600
comments: true
categories: [centos, virtualbox]
---

download centos6.6.iso from http://wiki.centos.org/Download x86_64

open virtualbox and create a new virtual machine 
name: centos-6.6
type: linux
version: Red Hat (64 bit)
memory: 512MB
harddrive: virtual
drivetype: vdi
storage: dynamically allocated
maxsize: 8GB

SETTINGS

NETWORK
Attached to: NAT
MAC Address: 080027F9414F

STORAGE
controller:ide > add cd/dvd device > choose disk
select centos6.6.iso

AUDIO
disable

USB
disable controller

NOW INSTALL

install centos, root password "vagrant"

restart and login as root

rm /etc/udev/rules.d/70-persistent-net.rules
cd /etc/sysconfig/network-scripts
cp -a ifcfg-eth0 ifcfg-eth1
vi ifcfg-eth1

DEVICE=eth1
HWADDR=08:00:27:F9:41:4F
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=dhcp

ifup eth1

restart and login as root

yum -y update
yum -y install openssh-server




