---
layout: post
title: "Git Snippets"
date: 2015-02-07 13:11:08 -0600
updated: 2015-02-07 13:11:08 -0600
comments: true
categories: [git]
---

#### Sort branches by last commit
``` bash
git for-each-ref --sort=-committerdate --format='%(color:yellow)%(committerdate:short)%(color:reset) %(refname)' refs/heads refs/remotes
```
