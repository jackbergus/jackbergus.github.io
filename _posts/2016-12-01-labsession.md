---
layout: post
title: Databases 2016 – 1st December
tags: teaching
---

# Installing MySQL
This tutorial is going to guide you throughout the setup of your workspace. First, we're going to see how to install the MySQL RDBMS in your preferred OS. This is going to be the only part that is OS dependent.

## Mac OS
For this tutorial we're going to use the **brew** package manager. The reason behind this is that we want to deal with a system that can be easily updated (we do not want to have different versions of the same db). So, you must first be sure that brew is actually installed in your Mac. Please visit the website [brew.sh](brew.sh) and follow the instructions. 

Now we're ready to kill all the processes and to completely remove MySQL (some traces could be left behind!).

    #!/bin/bash
    sudo killall mysql
    sudo killall mysql
    brew remove mysql
    brew cleanup
    sudo rm /usr/local/mysql
    sudo rm -rf /usr/local/var/mysql
    sudo rm -rf /usr/local/mysql*
    sudo rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
    sudo rm -rf /Library/StartupItems/MySQLCOM
    sudo rm -rf /Library/PreferencePanes/My*
    launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
    sudo rm -rf ~/Library/PreferencePanes/My*
    sudo rm -rf /Library/Receipts/mysql*
    sudo rm -rf /Library/Receipts/MySQL*
    sudo rm -rf /private/var/db/receipts/*mysql*
    
Now you can install MySQL by just running the following command:

    sudo brew install mysql
    
The default user is called `root`. In order to change its password and remove unsafe settings, call the following command:

    mysql_secure_installation
    
If the root user already has a password, then change the aforementioned command to `mysql_secure_installation -p` and then type the password. Now, depending on your OS setup, there could be some problems:

* If there are any pw problems, then probably the install setted automatically setted the password that is given in `/Users/<yourusername>/.mysql_secret`. Use that password
* If mysql cannot read the tmp lock file, this probably means that you did not start the server. In order to do so please type:

      brew services start mysql


Now MySQL could be accessed by typing the following command:

    mysql -u root -p
    
If you ignored the `mysql_secure_installation` setup, then just type `mysql -u root`. Please remember that new users and passwords could be set using some default commands. 

## GNU/Linux

For GNU/Linux systems you have to install both the mysql server (the actual database) and the client (the client where to send the SQL queries via terminal). 

    sudo apt-get install mysql-server mysql-client

By doing so, the system will automatically ask you the password of choice.

![Italian screen for password setup](http://jackbergus.alwaysdata.net/1.png)

# Set up a default database 

In this tutorial we're going to use the mysql test database provided at https://github.com/datacharmer/test_db. Please not that, since now you setted a password, the default command is changed to:

    mysql -u root -p < employees_partitioned.sql
    
Now we have to check if the procedure went smoothly. In order to do so, enter the MySql client:

    mysql -u root -p 

then we can see all the databases currently listed:

    > show DATABASES;
    
our database is called `employees`. So now you have to type:

    > use employees;

now you can list all the tables inside this database by typing:

    > show tables;

We can see each table schema with the command `describe`.

# Accessing RBDMSs through OO Languages

## JDBC

Each DB framework uses JDBC as a common interface for accessing to the relational database. This means that you must add the MySQL driver for the database

    <dependency>
        <groupId>org.clojure</groupId>
        <artifactId>java.jdbc</artifactId>
        <version>0.6.1</version>
    </dependency>
    <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>6.0.5</version>
     </dependency>

## jOOQ

Now create a Maven project with your favourite IDE, and add the following dependencies for your database. 

    <dependency>
      <groupId>org.jooq</groupId>
      <artifactId>jooq</artifactId>
      <version>3.8.6</version>
    </dependency>
    <dependency>
      <groupId>org.jooq</groupId>
      <artifactId>jooq-meta</artifactId>
      <version>3.8.6</version>
    </dependency>
    <dependency>
      <groupId>org.jooq</groupId>
      <artifactId>jooq-codegen</artifactId>
      <version>3.8.6</version>
    </dependency>

## Using a Persitency Framework in Java.  
   1. [Tutorial](https://github.com/jackbergus/javahibernateexample)
   2. Madhusudhan Konda: *Just Hibernate* O’Reilly Media. [Online book](https://www.safaribooksonline.com/library/view/just-hibernate/9781449334369/)
