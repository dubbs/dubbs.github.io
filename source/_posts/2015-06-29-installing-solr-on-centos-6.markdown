---
layout: post
title: "Installing Solr 4 on CentOS 6"
date: 2015-06-29 11:40:14 -0600
modified: 2015-07-03 11:20:14 -0600
comments: true
categories: [centos, solr]
---

## Verify CentOS Version
```bash
cat /etc/centos-release
```

## Install Tomcat
```bash
# install java-1.7.0-openjdk
sudo yum -y install java-1.7.0-openjdk
# install tomcat6
sudo yum -y install tomcat6 tomcat6-webapps tomcat6-admin-webapps
# change port
sudo sed -i s/8080/8983/g /etc/tomcat6/server.xml
```

To use the manager webapp you need to create a user with the role "manager".  Add a user to `<tomcat-users>` in `/etc/tomcat6/tomcat-users.xml`

```xml
<role rolename="manager" />
<role rolename="admin" />
<user username="admin" password="password" roles="manager,admin" />
```

Start tomcat

```bash
sudo service tomcat6 start
```

Open the [Tomcat Web Application Manager](http://admin:password@localhost:8983/manager/html)

## Install Solr
```bash
wget http://archive.apache.org/dist/lucene/solr/4.6.1/solr-4.6.1.tgz
tar xvf solr-4.6.1.tgz
# install solr app
sudo cp -a solr-4.6.1/dist/solr-4.6.1.war /var/lib/tomcat6/webapps/solr.war
# copy default example config to home, where data will live
sudo cp -a solr-4.6.1/example/solr /home/
# set appropriate permissions
sudo chown -R tomcat:tomcat /home/solr /var/lib/tomcat6/webapps/solr
# update solr home directory in WEB-INF
#    <env-entry>
#       <env-entry-name>solr/home</env-entry-name>
#       <env-entry-value>/home/solr</env-entry-value>
#       <env-entry-type>java.lang.String</env-entry-type>
#    </env-entry>
sudo vim /var/lib/tomcat6/webapps/solr/WEB-INF/web.xml
# restart so tomcat creates directory
sudo service tomcat6 restart
```

## Create a new core
```bash
cd /home/solr
sudo cp -a collection1 document_library
# update core name
echo 'name=document_library' | sudo tee document_library/core.properties
# clean data directory
sudo rm -rf document_library/data
sudo mkdir document_library/data
sudo chown -R tomcat:tomcat document_library
```

## Add drupal solr configuration to new core
```bash
sudo cp /vagrant/htdocs/sites/all/modules/apachesolr/solr-conf/solr-4.x/* /home/solr/document_library/conf/
sudo chown -R tomcat:tomcat document_library
sudo service tomcat6 restart
```

