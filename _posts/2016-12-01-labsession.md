---
layout: post
title: Databases 2016 – 1st December
tags: teaching
---

# How to setup your computer for the project
This tutorial is going to guide you throughout the setup of your workspace. First, we're going to see how to install the MySQL RDBMS


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

# Accessing RBDMSs through OO Languages

## Using a Persitency Framework in Java.  
        1. [Tutorial](https://github.com/jackbergus/javahibernateexample)
        2. Madhusudhan Konda: *Just Hibernate* O’Reilly Media. [Online book](https://www.safaribooksonline.com/library/view/just-hibernate/9781449334369/)
