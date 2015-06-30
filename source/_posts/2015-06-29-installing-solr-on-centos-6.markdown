---
layout: post
title: "Installing Solr 5 on CentOS 6"
date: 2015-06-29 13:40:14 -0600
modified: 2015-06-29 13:40:14 -0600
comments: true
categories: [centos, solr]
---

## Install Java
```bash
# install java-1.7.0-openjdk
sudo yum -y install java-1.7.0-openjdk
```

## Install Solr 5
```bash
# http://lucene.apache.org/solr/downloads.html
wget http://archive.apache.org/dist/lucene/solr/5.0.0/solr-5.0.0.tgz
tar zxf solr-5.0.0.tgz
cd solr-5.0.0/
bin/solr start
bin/solr create -c demo
```
