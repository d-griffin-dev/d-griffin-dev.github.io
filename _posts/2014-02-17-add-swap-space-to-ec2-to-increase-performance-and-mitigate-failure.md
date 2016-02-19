---
layout: post
title: "Add Swap Space to EC2 to Increase Performance and Mitigate Failure"
date: 2014-02-17
categories: "Digital Humanities"
tags:
- AWS
- MySQL
- Omeka
- Terminal
- WordPress
---

# Add Swap Space to EC2 to Increase Performance and Mitigate Failure

I didn't realize this until it was too late: AWS EC2 instances do not come with a swap partition. So what does this mean? Well, it explains (1), why my WordPress installation was randomly crashing when I saved a post, and (2), why my instance of Omeka crashed during a tutorial I was giving on the platform–which was really not a lot of fun, let me tell you. The process I describe below explains how the problem was found and how to mitigate this sort of failure by installing a swap file (I'll be using Ubuntu 12 LTS, but it should work on any Linux machine).

So, I had mentioned in [an earlier post][1] that I was having problems occasionally with WordPress giving me a database error, and that I could solve this pretty easily by just restarting the mysql service (`sudo service mysql restart`). But this problem happened so rarely that I didn't really expend much effort to figure out what was going on. But when my Omeka install crashed while about 20 people were trying to log in, and it locked up so no one could even view the front end, I knew something was up. After sputtering through the rest of the presentation using the Digital Faust site as emergency backup, I went back to my computer to try to figure out what locked up the site. On a whim, I tried restarting the mysql server, and that fixed everything up. So, it was just a matter of looking into the mysql error logs to find the following Fatal Error:

    InnoDB: Fatal error: cannot allocate memory for the buffer pool

So, aha, that is probably what was going on. Some quick googling led me to an answer at [Stack Overflow][2], and then I looked at [one or two other sites][3] to make sure that I had a good grasp of the problem: lack of swap space.

#### Intro to Swap

Basically, Unix systems use the term "swap" to signify the moving of memory from RAM to the hard drive, and back and forth (hence the term; virtual memory is another similar term). The swap space is where on the hard drive this swapping back and forth actually takes place. Usually, when one installs a Linux instance, the option to have a hd partition dedicated to swap is a default option. However, when using a micro EC2 instance in AWS, there is no swap space by default. I didn't know this, and I had little reason to guess there wasn't any, given the statistics that pop up when you login to the instance:

    System load: 0.08              Processes: 74
    Usage of /: 18.4% of 7.87GB    Users logged in: 0
    Memory usage: 77%              IP address for eth0: xx.xx.xxx.xxx
    Swap usage: 0%

So, looking at those statistics, one might be inclined to think as I did, "OK, my swap usage is 0%, which makes sense because I have just booted up and there has been no reason to use it yet, but because I have a percentage there, it must exist." Well, it turns out that just isn't true for the micro instance. If I had used the command `swapon -s` to check the swap statistics, I would have seen that there is nothing going on; `free -k` is another option, which put out the following:

                 total    used    free   shared   buffers    cached
    Mem:        604340  588772   15568        0       720     26196
    -/+ buffers/cache:  561856   42484
    Swap:            0       0       0

That's a whole lot of zero!

#### Creating a Swap File

Because its not a good idea to repartition a drive when its already in use, there is another option: designate a file to be used for swap instead of a whole partition. The first step is to create a swap file, which can be done with the following command:

    sudo dd if=/dev/zero of=/swapfile bs=1M count=1024

The `dd` command takes an input file (`if=_file_`) and copies it to an output file (`of=_file_`). It is generally used for copying whole partitions, as it goes block by block, which means its a bit slower than some other methods, but it useful for this sort of task. For this command, I am using a blank device (`/dev/zero` gives nothing but a string of zeros) and creating the swapfile in return. [A good rule of thumb][4] for swap is twice the size of physical memory–a micro instance at this writing has 0.615GB. I am setting the size at at 1GiB: it is a little bit less than twice, but I don't want to fill up the internal storage and the 1GiB should be more than enough for our purposes. This command takes a few seconds to run, and puts out the following if all goes well:

    1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 36.9033 s, 29.1 MB/s

#### Enabling Swap

Now that we have created the swap file, we just need to activate it. We start by making a swap space with the following command:

    sudo mkswap /swapfile

What the `mkswap` command did was set our swapfile as the swapspace for the system. I got the following output:

    Setting up swapspace version 1, size = 1048572 KiB
    no label, UUID=c6810168-a5aa-4a74-bcd7-1502942e2667

Now we enable the swapspace with the `swapon` command:

    sudo swapon /swapfile

There was no output for that command, but by typing `swapon -s` I can see that I now have a swapspace:

    Filename    Type    Size       Used  Priority
    /swapfile   file    1048572    0     -1

One last thing to do: I have to enable the swapfile in file systems table (`fstab`) so that on a reboot the swapfile is enabled. That file is at `/etc/fstab`; you can edit it with the following command:

    sudo nano /etc/fstab

`Nano` is a straightforward editor, so that's why I use it here. There is probably only one line in this file; on a new line, paste in the following:

    /swapfile    none    swap sw 0 0

Save it and exit (`ctrl-O` and then `ctrl-X`). The swap is now enabled and from now on.

#### Some further options

There are two more steps you should probably take: adjust the "swappiness" and lock down the swap file. I love the term swappiness: I thought it was made up the first time I saw it, but it is in fact [an accepted term][5]. Swappiness is a ratio of how often the system will write to the swapfile: if set to zero, the system will only swap to avoid running out of memory (the error above); if set to 100, the system will attempt to swap all the time. The default is set at 60. Since we want to utilize the swap only when necessary, we can set the swappiness to zero with the following commands:

    echo 0 | sudo tee /proc/sys/vm/swappiness
    echo vm.swappiness = 0 | sudo tee -a /etc/sysctl.conf

The next step is locking down the swapfile so other users won't play with it. If you are the only user you likely won't have a problem, but it is a good best practices sort of thing to do. You can do that with the following commands:

    sudo chown root:root /swapfile
    sudo chmod 0600 /swapfile

And that's it! You now have your own swap space set up, optimized, and locked down.

[1]: http://danielgriff.in/2014/wordpress-error-establishing-a-database-connection-fixed/ "WordPress Error Establishing a Database Connection — Quick Fix"
[2]: http://stackoverflow.com/questions/10284532/amazon-ec2-mysql-aborting-start-because-innodb-mmap-x-bytes-failed-errno-12 "Amazon EC2, mysql aborting start because InnoDB: mmap (x bytes) failed; errno 12"
[3]: https://www.digitalocean.com/community/articles/how-to-add-swap-on-ubuntu-12-04
[4]: http://superuser.com/questions/16280/swap-partition-size-for-4gb-ram "Swap partition size for 4GB RAM"
[5]: http://en.wikipedia.org/wiki/Swappiness "Wikipedia: Swappiness"
