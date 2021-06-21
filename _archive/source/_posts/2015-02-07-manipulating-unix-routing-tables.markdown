---
layout: post
title: "Manipulating Unix Routing Tables"
date: 2015-02-07 12:25:20 -0600
updated: 2015-02-07 12:25:20 -0600
comments: true
categories: [unix, mac]
---

For one of our latest projects, we needed to restrict incoming connections on the destination server prior to launching the our website.

So, we had our developers route their traffic through a VPN gateway by adding entries to the routing table:

``` bash
sudo route -nv add IP_DESTINATION IP_GATEWAY
```

You can verify the route was added by using `netstat`

``` bash
netstat -rn
# Routing tables
#
# Internet:
# Destination        Gateway            Flags        Refs      Use   Netif Expire
# IP_DESTINATION     IP_GATEWAY         UGHS            0        0     en0
```

You can verify the connection is going through the vpn by using `traceroute`

``` bash
traceroute IP_DESTINATION
# traceroute to IP_DESTINATION, 64 hops max, 52 byte packets
#  1  IP_GATEWAY  1.586 ms  2.257 ms  0.885 ms
```

You can remove the route by using the delete option

``` bash
sudo route delete IP_DESTINATION
# delete host IP_DESTINATION
```
