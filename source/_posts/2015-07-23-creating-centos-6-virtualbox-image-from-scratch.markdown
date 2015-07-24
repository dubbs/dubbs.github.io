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
Port Forwarding: host 2229 guest 22

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

vi /etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE=eth0
HWADDR=08:00:27:F9:41:4F
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=dhcp

ifup eth0

restart and login as root

rpm -Uvh http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
yum -y --enablerepo rpmforge install dkms
yum -y groupinstall "Development Tools"
yum -y install kernel-devel
yum -y update
yum -y install openssh-server
# install guest additions
wget http://download.virtualbox.org/virtualbox/4.3.30/VBoxGuestAdditions_4.3.30.iso
mkdir -p /mnt/iso
mount -t iso9660 -o loop VBoxGuestAdditions_4.3.30.iso /mnt/iso
cd /mnt/iso
./VBoxLinuxAdditions.run
cd
umount /mnt/iso


# install user

/usr/sbin/adduser vagrant
passwd vagrant
set as vagrant

/usr/sbin/visudo
vagrant ALL=(ALL) NOPASSWD: ALL

vi /etc/ssh/sshd_config
UseDNS no
PermitRootLogin no
AllowUsers vagrant

su - vagrant
mkdir .ssh
curl https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub > .ssh/authorized_keys
chmod 0700 .ssh
chmod 0600 .ssh/authorized_keys


exit

on host download private vagrant key to .ssh, chmod 0600

Host centos
HostName 127.0.0.1
User vagrant
Port 2229
IdentityFile ~/.ssh/vagrant

test ssh
ssh centos

# package
vagrant package --base centos-6.6


