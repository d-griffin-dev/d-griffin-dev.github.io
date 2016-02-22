---
layout: post
title: "Creating beautiful timelines with TimelineJS"
date: 2014-04-10
categories: "Digital Humanities"
tags:
- "Knight Lab"
- "Digital Faust"
- SIMILE
- time
- TimelineJS
---

For several years I have been using [SIMILE timelines][1] in a variety of applications because it is very straightforward and simple to code. However, one thing it is not is sexy: it takes a lot of work to make your timelines look pretty, and adding anything else besides text can be a chore. But I recently had the opportunity to work with a new tool, [TimelineJS from the Knight Lab at Northwestern University][2], that is even easier to use and has a really sleek, intuitive, and (dare I say it) fun interface right out of the box. Here I am going to briefly describe TimelineJS and give you an example of how I am using it on my Digital Faust site.

The Knight Lab has put out a lot of great products recently, and you should definitely check out their tools and see if you can put them to use in your own projects. The mission of the lab is to create digital tools for journalists, but many of them have obvious relevance for many digital humanities projects. In the coming days I will be writing a few more articles on their tools, so look forward to those.

TimelineJS gives you the ability to easily create an attractive timeline with images, links, and videos. What makes it so easy is that all you have to do is put in all your information into a Google spreadsheet. Once you have finished filling in the fields (date, title, image link, credits, etc.), you publish the spreadsheet and insert a small bit of code (iframe) into your HTML. And that's it! The tool also allows for HTML coding, so you can easily write links into the timeline. I also like that the date field is very forgiving and doesn't require strict coding–very handy when you are dealing with whole years as your dates, which is usually the case with books.

Below is a simple implementation that is straight from the [Digital Faust Project website][3]. We found that Neatline wasn't really cutting it for our purposes, as its geographic functions were really just unnecessary, and a simile timeline was getting too bogged down with all the items. What we wanted was a way to view our images and be able to navigate through them with a timeline. TimelineJS is serving this purpose marvelously.

<iframe src="http://cdn.knightlab.com/libs/timeline/latest/embed/index.html?source=0Aj1FHvEQjbvKdFhPa0VRb1hZMEpIc1ZPWlY1S01JNHc&amp;font=Bevan-PotanoSans&amp;maptype=toner&amp;lang=en&amp;height=650" width="100%" height="650" frameborder="0" data-origwidth="100%" data-origheight="650" style="width: 663px;"></iframe>

There are just a few downsides. Customizing the interface can be a little difficult, but luckily it is very pretty right out of the box. As we might expect, it is also somewhat difficult to deal with BCE dates, but that is the case with almost all digital timeline tools. Also, it is pretty easy to figure out where your spreadsheet is, so you will have to make sure to lock it down with privacy settings. All in all, though, it's a pretty cool tool that I can heartily recommend.

[1]: http://www.simile-widgets.org/timeline/ "SIMILE Widgets | Timeline"
[2]: http://timeline.knightlab.com/ "Timeline JS"
[3]: http://digitalfaust.lib.duke.edu/timeline "Digital Faust Project | Timeline"
