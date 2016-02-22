---
layout: post
title: "Mitigation Strategies for a XMLRPC.PHP attack"
date: 2014-07-17
categories: "Digital Humanities"
tags:
- attacks
- WordPress
---

I haven't been able to get easy access to a terminal in the last month or so, I have been pretty flustered when it comes to the performance of this site. If you haven't noticed, it is going up and down—more down than up I suspect, unfortunately, because I don't check on it every day to reset it if its down. But today I was able to get in and poke around a bit in the access logs to see what's been going on. That's when I noticed this strange behavior: excessive use of the `xmlrpc.php` file.

Looking at the Apache log files, I would see entries that looked something like this:

    danielgriff.in:80 80.82.78.57 - - [14/Jul/2014:07:02:36 +0000] "POST /xmlrpc.php HTTP/1.0" 500 610 "-" "Mozilla/4.0 (compatible: MSIE 7.0; Windows NT 6.0)"

And there were many other similar entries.  The IP address here points to a rather suspicious location—you can Google it and see how [disreputable][1] it actually is. The interesting thing here is what is being asked: `"POST /xmlrpc.php HTTP/1.0" 500 610`. In case you are wondering, this is a really odd file for anybody to be querying.  You can read what xmlrpc.php is used for in it's [WordPress Codex][2] entry; basically, it allows clients to make changes to their WordPress sites using a method other than the web interface—say, using your iPhone app or a desktop application.

Unfortunately, this file can be abused by nasty folks. There is a good summary of how attackers can exploit the pingback function in the xml-rpc library on this [Acunetix page][3]. To summarize, they can use it to (1) to guess hosts inside the internal network and (2) subsequently port scan those hosts, (3) carry out a DDOS attack, or (4) attack the login credentials of an internal server. If I had to guess, the 3rd option is likely the one that was affecting my site, but that's a guess.

So, what can one do? I am going to give you a couple of options. I'll get back to you on the effectiveness of the solutions at a later point in time.

1\. Deny access to the file in the Apache .htaccess file (from [Vilpponen][4])


    Order allow,deny
    Deny from all


2\. Send requests to 0.0.0.0, using the Apache .htaccess file (goldenguineas on the [WordPress Support Forums][5])

    RewriteRule ^xmlrpc.php$ "http://0.0.0.0/" [R=301,L]

3\. Block IP addresses, using the following linux command (Purab Kharat on the [WordPress Support Forums][6])

    iptables -A INPUT -s 198.154.62.21 -j DROP

4\. Block the agent making the attack (Tigr on the [WordPress Support Forums][5])

    # Block attackers by agents

    RewriteCond %{HTTP_USER_AGENT} ^.*WinHttp.WinHttpRequest.5.*$
    RewriteRule .* http://%{REMOTE_ADDR}/ [R,L]


NOTE: Remember, blocking access to xmlrpc.php will block some WordPress features–the biggest one I can see is Jetpack.

Feel free to make further suggestions. Again, I'll update on what effect this has for me.

**UPDATE 7/21/2014:** A whole weekend without any down time! This is probably solving all my issues. Score!



[1]: https://www.virustotal.com/en/ip-address/80.82.78.57/information/ "80.82.78.57 IP address information - VirusTotal"
[2]: http://codex.wordpress.org/XML-RPC_Support "XML-RPC Support"
[3]: http://www.acunetix.com/blog/web-security-zone/wordpress-pingback-vulnerability/ "WordPress Pingback Vulnerability Found in WordPress 3.5"
[4]: http://antti.vilpponen.net/2013/08/26/how-to-mitigate-a-wordpress-xmlrpc-php-attack/ "How to mitigate a WordPress XMLRPC.php attack"
[5]: http://wordpress.org/support/topic/resolving-xmlrpcphp-ddos-attack-with-htaccess-redirect "Resolving XMLRPC.PHP DDOS attack with htaccess redirect?"
[6]: http://wordpress.org/support/topic/xmlrpcphp-attack-on-wordpress-38 "xmlrpc.php attack on WordPress 3.8"
