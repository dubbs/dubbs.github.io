---
layout: post
title: "How to Create a CentOS 6 VirtualBox Provider for Vagrant"
date: 2015-07-23 11:40:14 -0600
modified: 2015-07-23 11:40:14 -0600
comments: true
categories: [centos, virtualbox, vagrant]
---

## Install VirtualBox 4.3.30 on host
```bash
# https://www.virtualbox.org/wiki/Download_Old_Builds_4_3
wget http://download.virtualbox.org/virtualbox/4.3.30/virtualbox-4.3_4.3.30-101610~Ubuntu~raring_amd64.deb
sudo dpkg -i virtualbox-4.3_4.3.30-101610~Ubuntu~raring_amd64.deb
```

## Download CentOS 6.6 ISO
```bash
# http://wiki.centos.org/Download
wget http://mirror.its.dal.ca/centos/6.6/isos/x86_64/CentOS-6.6-x86_64-minimal.iso
```

## Create base VirtualBox VDI
1. Create a new virtual machine in VirtualBox
	Name: centos-6.6
	Type: linux
	Version: Red Hat (64 bit)
	Memory: 512MB
	HD: new virtual drive
	DriveType: VDI
	Storage: dynamically allocated
	Maxsize: 8GB

2. Configure settings
	Storage:
		Click add CD/DVD Device beside Controller: IDE	
		Choose disk: find and select CentOs-6.6-x86_64-minimal.iso
	Audio:
		Disable
	Network:
		Attached to: NAT
	USB:
		Disable

3. Install OS
	root password: vagrant

## Remove GRUB timeout on guest
```bash
vi /boot/grub/grub.conf
timeout=0
```

## Initialize network on guest
```bash
vi /etc/sysconfig/network-scripts/ifcfg-eth0
ONBOOT=yes
# start eth0
ifup eth0
```

## Install Guest Additions on guest

Install updates
```bash
yum -y update
```

Install dkms so virtualbox-dkms module is recompiled with every new kernel
```bash
rpm -Uvh http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
yum -y --enablerepo rpmforge install dkms
```

Install the CentOS development tools so we can add virtualbox to the kernel
```bash
yum -y groupinstall "Development Tools"
yum -y install kernel-devel
```

Restart to get new kernel headers
```bash
shutdown -r now
```

Install Guest Additions
```bash
curl http://download.virtualbox.org/virtualbox/4.3.30/VBoxGuestAdditions_4.3.30.iso > vboxguest.iso
mkdir -p /mnt/iso
mount -t iso9660 -o loop vboxguest.iso /mnt/iso
cd /mnt/iso
./VBoxLinuxAdditions.run
cd
umount /mnt/iso
```

## Create vagrant user on guest
```bash
# add user
/usr/sbin/adduser vagrant
passwd vagrant
# add sudo
/usr/sbin/visudo
vagrant ALL=(ALL) NOPASSWD: ALL
# add insecure key
su - vagrant
mkdir .ssh
curl https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub > .ssh/authorized_keys
chmod 0700 .ssh
chmod 0600 .ssh/authorized_keys
exit
```

## Configure SSH on guest
```bash
vi /etc/ssh/sshd_config
# set following
UseDNS no
PermitRootLogin no
AllowUsers vagrant

vi /etc/sudoers
# comment out following
#Defaults requiretty

service sshd restart
```

## Free up space on guest
```bash
yum clean all
dd if=/dev/zero of=/bigemptyfile bs=4096k
rm -rf /bigemptyfile
```

## Compact VDI on host
Shutdown VM and use vboxmanage to compact image
```bash
# ensure VM is shutdown
cd VirtualBox\ VMs/centos-6.6/
vboxmanage modifyhd centos-6.6.vdi --compact
```

## Create vagrant box
```bash
vagrant package --base centos-6.6
vagrant box add centos-6.6 package.box
vagrant init centos-6.6
vagrant up
vagrant ssh
```

## Configure Vagrantfile
```ruby
config.vm.provider "virtualbox" do |vb|
  vb.memory = "1024"
  vb.cpus = 2 
end 
```



