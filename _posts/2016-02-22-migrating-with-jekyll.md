---
layout: post
title: "Migrating with Jekyll"
date: 2016-02-22
categories: Announcements
tags:
- blog
- Jekyll
---

So, after many months of experimentation and false starts, I have finally migrated to a Jekyll static site blog, shuttering the WordPress version completely.

I found that WordPress was more than I needed and the care and uptake was not worth the cost of keeping the server up. I have not been contributing much nor ever really have much in the way of visitors, so no technical necessity to have a behemoth of a CMS to power the operation.

So what you may see now is the first deployed version of this site. If it looks like the static template for Jekyll, that is probably because it is. A newer practice for me in my technical projects is to launch first, enhance later—less paralysis by over design. If something gets really big I can maybe get some funding to refactor in a big way, but for my purposes right now this will do just fine.

The process of migrating to Jekyll was not too difficult, it just took me a bit of time to figure out the quirks:

1. **Retaining the Link Structure:** I wanted to retain my link structure from the old site, so that required a bit of tinkering with the `_config.yml` settings. The hard part was figuring out which setting I needed to set; once i figured out it was permalink (and that I needed to `jekyll build` after a settings change), it was pretty easy to set it to `permalink: "/:year/:title/"`.

2. **Figuring out the post frontmatter:** The YAML frontmatter took me longer to figure out than I care to admit, mainly that listing out my categories and tags was not working as I thought it would, but rather that I needed to do an unordered list (- item) for it to work as I intended.

3. **Making sure that I had the proper "server" configuration:** Not too much more to say here, I just had to take a bit of time configuring my CNAME / A records and my DNS took longer than expected.

4. **Converting posts to Markdown:** This wasn't too hard, just tedious. I took advantage of a markdown conversion service, [heckyesmarkdown.com](http://heckyesmarkdown.com), with some of the labor. Incidentally, there were fewer of these conversion services available than I thought there would be. The code blocks are still broken though (ugh).

5. **Images:** I have to go back and manually add images because I couldn't find a quick and efficient way to import them (ugh again).

I will work on upgrading the site over the next few months, but right now the focus is on writing more. I feel comfortable I can upgrade any functionality I may need \(if I feel like I need comments, I might add a connection to Firebase\), so let's get cracking on the thinking work! A new job is on the horizon, I hope I can keep up the momentum.
