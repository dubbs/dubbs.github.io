---
layout: post
title: "Forward vs Reverse Proxy using Apache"
date: 2013-09-23 13:09
comments: true
categories: apache
---

A proxy server acts as an intermediary between a client and a destination server, primarily controlling access to resources and caching content.
 
To enable proxy functionality in Apache, use the following:

	# /etc/apache2/httpd.conf
	LoadModule proxy_module modules/mod_proxy.so
	LoadModule proxy_http_module modules/mod_proxy_http.so
 
## Forward Proxy
 
A [Forward Proxy](http://en.wikipedia.org/wiki/Proxy_server#Forward_proxies) is typically used to:
 
- hide the identities of clients
- provide internet access to clients blocked by firewall
- cache content to reduce network traffic
 
A forward proxy accepts requests from internal clients and forwards them to an external resource.

{% img /images/forward-proxy.png %}
 
To configure this in Apache, we can simply control who has access to external resources by
specifying their IP address.
 

	<VirtualHost *:80>
	  # Enables Apache to acting as forwarding server
	  ProxyRequests On
	 
	  # Adds Via header to requests
	  ProxyVia On
	  
	  <Proxy *>
	    Order deny,allow
	    Deny from all
	 
	    # Only allows this client to proxy requests
	    Allow from 192.168.0.11
	  </Proxy>
	</VirtualHost>


## Reverse Proxy
 
A [Reverse proxy](http://en.wikipedia.org/wiki/Reverse_proxy) is typically used to: 
 
- hide the identities of servers
- act as load balancer
- act as web accelerator
- used to perform A/B testing
 
A reverse proxy accepts requests from external clients and usually forwards them
to an internal resource.
 
{% img /images/reverse-proxy.png %}
 
To configure this in Apache, we use the `ProxyPass` and `ProxyPassReverse` directives.

	<VirtualHost *:80>
	  # Prevents Apache from acting as forwarding server
	  ProxyRequests Off
	 
	  # http://example.com/assets/css to be internally converted into a proxy request to http://cdn.example.com/css
	  ProxyPass /assets/ http://cdn.example.com/
	  
	  # if http://cdn.example.com/css has a redirect to http://cdn.example.com/v2/css
	  # Apache will adjust the request to http://example.com/assets/v2/css before forwarding response
	  ProxyPassReverse /assets/ http://cdn.example.com/
	</VirtualHost>
