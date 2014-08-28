---
layout: post
title: "Apache and or Nginx"
date: 2014-03-14 23:54
comments: true
categories: [apache, nginx]
---

## Apache

Apache was designed to process web requests in a **consistent** and **reliable** manner.

It uses a **process-driven** architecture and handles one connection per-process or per-thread.

Apache comes with 2 multi processing modules (MPM), which bind to ports, accept requests and spawn processes:

1. **Prefork** MPM

	Uses multiple child processes with one thread each. 
	Each process handles one connection at a time.

2. **Worker** MPM

	Uses multiple child processes with many threads each.
	Each thread handles one connection at a time.
	If you want to use mod_php (not thread safe), it is recommended that you also use FastCGI, so that php is running in its own memory space.

Apache does not scale well under high server load, often consuming large amounts of RAM and CPU.

With Apache, it is easy to configure complex setups, it has excellent documentation and abundant module availability.

## Nginx

Nginx was designed to solve the [c10k problem](https://en.wikipedia.org/wiki/C10k_problem), and as a result, is a highly performant http and reverse proxy server.

It uses an **event-driven** architecture and handles multiple connections
in a single event loop.

Nginx is very good at serving *static content*, which accounts for 80 - 95% of website requests.
It scales very well under high server load, and is not dependent on the underlying server hardware.

Compared to other more established web servers, 
complex site configuration is typically more difficult with Nginx, 
due to lightweight design.  It also lacking in documentation and module support.

Some notable sites using Nginx include:

- github
- wordpress
- pinterest
- netflix
- cloudflare

## Both?

Nginx can be used as a reverse proxy, in front of Apache, handling all 
requests for static content.  All other requests are forwarded to 
Apache via `proxypass`.  You will need to install mod_rpaf, so 
Apache can pick up the `X-Real-IP` header provided by nginx.  This will make
it seem as though Apache handled the original request.



<!--
http://www.stderr.net/apache/rpaf/
http://serverfault.com/questions/273260/when-should-i-switch-to-nginx
http://www.inetu.net/about/server-smarts-blog/january-2013/nginx-vs-apache-which-web-server-is-right-for-your
 -->
