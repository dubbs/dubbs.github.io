---
layout: post
title: "Executing Drush Commands outside of Vagrant using Aliases"
date: 2014-09-18 11:17:42 -0600
updated: 2014-09-11 11:14:59 -0600
comments: true
categories: [drush, drupal, ssh]
---

Drush aliases make it possible to run commands from outside your
vagrant box, and even outside of your project directory.

To start, you need a `~/.drush/dev.aliases.drushrc.php` file:

```php                                                                         
$aliases['project1'] = array(
	'root' => '/vagrant', 
	'remote-host' => '10.0.0.124', 
	'remote-user' => 'vagrant', 
); 
$aliases['project2'] = array( 
	'root' => '/vagrant', 
	'remote-host' => '10.0.0.125', 
	'remote-user' => 'vagrant', 
);
```

Next, you need to copy your public key to your vagrant box, the password
is `vagrant`:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@10.0.0.124
```

And finally, test the connection:

```bash
drush @project1 status
```

Drush will use your alias configuration to select the appropriate root directory and remote
host.  You should be able to run the above command from any directory.
