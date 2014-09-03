---
layout: post
title: "Ignorning ri and RDoc during gem install"
date: 2014-08-22 11:56
updated: 2014-08-22 11:56
comments: true
categories: ruby
---

I personally never use the ri (Ruby Index) and RDoc (Ruby Documentation).

To prevent them from installing during `gem install`, just add this line to your ~/.gemrc or /etc/gemrc:

	gem: --no-rdoc --no-ri
