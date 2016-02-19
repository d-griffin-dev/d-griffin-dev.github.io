---
layout: post
title: "Finding Weird Stuff in the Access Logs"
date: 2014-07-28
categories: "Digital Humanities"
tags:
- "access logs"
---

# Finding Weird Stuff in the Access Logs

One of the jobs I have self-appointed myself as a Monday task is looking over the access logs for our various sites that we host. Our vendor doesn't provide the best tools for doing this, but it has proven a fairly interesting endeavor nonetheless. I am keeping track of some of these things in an informal log. Here are some weird things that I have noticed already.

### A Funny Looking Mirror

Today I noticed that for one of our sites something like 75% of our referrals were coming from one site, accounting for about 11% of all traffic. When you visited the site, it was actually a complete mirror of the original–the only thing different was the domain name (ourwebsite.com vs theirwebsite.com).

The first think I thought was, "oh great, somebody has ripped our entire site and put it up here." But browsing the site via proxy I realized that none of the restricted content was available–I would have thought that it would be if a hacker had put it up. So I did a WHOIS look up for the domain. I was expecting to get very little information, but I got something curious: the name and address of our web vendor. Curious. I did a traceroute on the domain and found that it actually pointed to our vendor's servers, the exact same one as our page. I looked at the access logs again: I could see mention of it all the way back to 2005, shortly after the site went live. So it would appear that it is just a mirror that someone left up (probably as a test for another journal, it would appear from the name) and forgot about. The only remaining question is why it is popping up now with such high usage–probably a crawler.

### A Cow in the Data

A couple of days ago, I ran a usage report on subscribers to one of our sites. Far and away the greatest number of hits were coming from one institution, which had more than five times the number of access events compared to the next user. This #2 institution was probably as high as it was because it was running a [LOCKSS scan][1] of the site, so this usage was off the charts compared to even that bot. The strangest thing was the name of the institution: a bookseller out of Europe. What were they doing with our stuff? Had they been hacked, and was someone using their access to hijack content?

So, I did some more digging. The name of the user, it turns out, was misleading: it was actually a large university out of Berlin. So, ok, what does someone want with our information in Berlin? I couldn't find any thematic connection between our collection and the university with a search of their faculty and research, so I googled something like "web crawler Berlin". I came across the website of a [web-crawler that is building a linguistic corpus of English language usage on the web][2]. I looked again at the access log reports to see what was there (in a different place from the usage statistics), and, sure enough, I found heavy usage from a machine registered to the research group in Berlin. Too bad for them the English that they found was probably not very current, since it dates back to the 19th century.

[1]: http://www.lockss.org/ "Lots of Copies Keep Stuff Safe"
[2]: http://hpsg.fu-berlin.de/cow/ "COW – Corpora from the web"
