---
layout: post
title: "Modernizr Plugin API"
date: 2014-03-13 16:22
comments: true
categories: js
---

Modernizr provides a great plugin API, where custom tests can be added, for features that are currently not supported.

To add a test, simply call the `addTest` method.

Here is a example of a test used to determine if a browser [cuts the mustard](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard):

	(function (window, document, Modernizr) {
	  'use strict';
	  Modernizr.addTest('mustard', function () {
		if('querySelector' in document &&
			'localStorage' in window &&
			'addEventListener' in window) {
		  return true;
		}
	  });
	}(this, document, this.Modernizr));
