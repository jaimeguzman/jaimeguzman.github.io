---
layout: post
title: Install Drush on CentOS or Any base Unix
date: "2015-06-27 21:25:06"
comments: true
published: true
---


Step by step


1. Check if you have root access
2. sudo yum install php-pear     (RedHat based)
3. pear channel-discover pear.drush.org 
4. pear install drush/drush
5. Check if Console table library is installed
    5.1 if yes just use drush on you root path of drupal installtion
    5.2 else install Console table: (more info)[https://www.drupal.org/node/2132447#comment-9740457]
       - sudo requiered
       - cd /usr/share/pear/drush/lib/
       - wget http://download.pear.php.net/package/Console_Table-1.1.3.tgz
       - tar -zxvf Console_Table-1.1.3.tgz
       - rm -fr *.tgz
       - run *drush*, test and use =)
