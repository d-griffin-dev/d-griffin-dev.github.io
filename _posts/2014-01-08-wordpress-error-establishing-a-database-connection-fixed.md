---
layout: post
title: "WordPress Error Establishing a Database Connection — Quick Fix"
date: 2014-01-08
permalink: /2014/wordpress-error-establishing-a-database-connection-fixed/
categories: "Digital Humanities"
tags:
- databases
- MySQL
- WordPress
---

**UPDATE: I think I found the solution to this problem: check out [this post][1] (and [this post][2]).**

I had a scary moment earlier today: my brand spanking new WordPress site decided to fail on me. It seemed implausible: one minute its working, and the next, poof! I was getting on every page the message "Error establishing a database connection". When I went to my WordPress admin page, I got a little bit more information:

> **Error establishing a database connection**
>
> This either means that the username and password information in your _wp-config.php_ file is incorrect or we can't contact the database server at localhost. This could mean your host's database server is down.
>
> * Are you sure you have the correct username and password?
> * Are you sure that you have typed the correct hostname?
> * Are you sure that the database server is running?
>
> If you're unsure what these terms mean you should probably contact your host. If you still need help you can always visit the WordPress Support Forums.

Yikes! Visiting the WordPress Support Forums is not a particularly fun idea in my book. So, I went searching and found this very helpful page on how to fix this particular error ([wpbeginner][3]). I used this as my reference, but I found the instructions geared more toward someone hosting on an external site. I knew that server had to be online (something had to be throwing the error messages), so I fired up the terminal and got a ssh connection with the server.

The first clue was checking out the `wp-config.php` file mentioned in the error. I thought it would be weird if anything had changed in here, and all the settings looked correct. It was mentioned that perhaps changing the entry in the line `define('DB_HOST', 'localhost');` from `'localhost'` to my own IP address would work, but it didn't for me. So, I figured I would try to see if there was something up with the MySQL server. When I tried running `mysql` from the terminal, I got the following error:

    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (111)

Ok, now this looked scary. But before I tried running that down, I decided to try restarting the MySQL server with the following command:

    sudo service mysql start

And bam, that did the trick (`restart` works as well). WordPress was up and running once again, all nice like. I have no idea what triggered the event. The only difference I noticed was that, once I had the WordPress running again, there was a theme update waiting for me to download, although I don't know for sure if that had been there already or not. But I'm glad I got everything back up and running again!

UPDATE 11 Jan 2014: I've noticed this happens occasionally when I submit a new post. I think it may be related to the Publicizing on Twitter, but I'm not sure.

UPDATE 21 Jul 2014: I was able to solve this problem by [adding swap space][1]; also of interest may be my discussion on [mitigating automated WordPress attacks][2].

[1]: http://danielgriff.in/2014/add-swap-space-to-ec2-to-increase-performance-and-mitigate-failure/ "Add Swap Space to EC2 to Increase Performance and Mitigate Failure"
[2]: http://danielgriff.in/2014/mitigation-strategies-for-a-xmlrpc-php-attack/ "Mitigation Strategies for a XMLRPC.PHP attack"
[3]: http://www.wpbeginner.com/wp-tutorials/how-to-fix-the-error-establishing-a-database-connection-in-wordpress/ "How to fix the Error Establishing a Database Connection"
