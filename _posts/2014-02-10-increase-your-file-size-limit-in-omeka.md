---
layout: post
title: "Increase your file size limit in Omeka"
date: 2014-02-10
categories: "Digital Humanities"
tags:
- "Digital Faust"
- Ubintu
- VMs
- VMware
---

I've seen a few queries in the Omeka documentation asking how to increase the file size limit for uploading files. I found that I needed to do this exact task this week, but I had to poke around a bit in the file system to get the right file. So, since the forum has information that is a bit older, I thought I write up briefly the steps you need to perform to increase that file size limit. The system I am using has Omeka 2.1.4 installed on an Ubuntu 12 server: the file system will likely be different if your OS is different.

Omeka in and of itself does not preclude large files: the main issue comes from default settings for PHP. Therefore, we need to alter some of the configuration files to up that limit. If you followed the instructions on the Omeka site for installation and thus are using PHP5 & Apache2, the file you need can be found on your server here:

    sudo nano /etc/php5/apache2/php.ini

Notice the `sudo`; you will have to have administrator access to change this file, which is probably the case if you are reading this. The `php.ini` file is a long one, and the two things we need are about halfway down. The first option to change is under the `Data Handling` section (line 740 in my documentation):

    ; Maximum size of POST data that PHP will accept.
    ; http://php.net/post-max-size
    post_max_size = 2M

The default setting here is likely 2M–change it to the size you want. This number is in bytes, so you can use K (kilo-), M (mega-) and G (giga-), or none at all for a byte-sized file limit. You don't want it too large though, definitely not over the 32-bit integer limit; I set mine to 20M. POST, one of the four HTML protocols, handles requests to write on the server; GET, on the other hand, is a request to read from the server. So, change this to whatever you want to set your limit. The next option we need to change is a little further down. The option to change is under the `File Uploads` section (line 891 for me):

    ; Maximum allowed size for uploaded files.
    ; http://php.net/upload-max-filesize
    upload_max_filesize = 2M

This is pretty self explanatory: change the 2M to your chosen file size limit. Now we just need to save the file and exit (ctrl-O, yes, ctrl-X is the sequence in nano). You have now adjusted what you need in PHP, but lets go a little further and tell our users what their limit is. We can do this by altering our Omeka `.htaccess` file. We can change this with the following command:

    sudo nano /var/www/.htaccess

This will bring us into Omeka's Apache configuration file. Scroll down to the bottom and add these two lines at the end of the file:

    php_value upload_max_filesize 20M
    php_value post_max_size 20M

Basically, what you have just done is instruct Omeka and Apache that your file size limit is now different. I put here 20M as my upload file sizes; if you decided on a different number, just change these numbers to whatever size you chose. Once you save the file, all that is left is to restart Apache, which you can do with this command:

    sudo service apache2 restart

And you are all set! Now, when you go to the file upload tab, you will see that your file upload max size is reflected above the file chooser. Enjoy!
