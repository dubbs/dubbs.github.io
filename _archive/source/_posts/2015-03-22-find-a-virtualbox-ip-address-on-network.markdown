---
layout: post
title: "Find a VirtualBox IP address on network"
date: 2015-03-22 12:33:34 -0600
modified: 2015-03-22 12:33:34 -0600
comments: true
categories: [vm]
---

If you've recently connected to your VM, it should be in the arp cache.  
Query the arp cache looking for a MAC address starting with `8:0:27`

```bash
arp -an | grep 8:0:27
# ? (192.168.0.19) at 8:0:27:d:f1:55 on en0 ifscope [ethernet]
```

If you don't see it, you need to do a bit more digging.

```bash
ifconfig
# en0:
# 	inet 192.168.0.10 netmask 0xffffff00 broadcast 192.168.0.255
```

Since my network has a 192.168.0.10 with a netmask of 255.255.255.0, 
I can preform a "ping sweep" to count available machines on the network.

```bash
nmap -sn 192.168.0.0/24
# or
nmap -sn 192.168.0.*
```

This is "more reliable than pinging the broadcast address because many hosts 
do not reply to broadcast queries".

```bash
ping 192.168.0.255
```

Now the arp cache should show the VM, if its running:

```bash
arp -an | grep 8:0:27
# ? (192.168.0.19) at 8:0:27:d:f1:55 on en0 ifscope [ethernet]
```

You can find more information on the device by preforming an aggressive scan

```bash
nmap -A -v 192.168.0.19
```

