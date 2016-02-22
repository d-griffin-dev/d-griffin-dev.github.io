---
layout: post
title: "How would I design a one year digital humanities graduate assistantship?"
date: 2014-01-10
categories:
- "Digital Humanities"
- "Higher Education"
tags:
- AWS
- CSS
- Gephi
- "graduate school"
- HTML
- "Humanities Writ Large"
- JavaScript
- Neatline
- Omeka
- Pleiades
- TEI
- Terminal
- WordPress
---

This question was posed to me this week by my supervisor. I and one other student are the first to hold a Digital Humanities Graduate Assistantship, so the program is still being tweaked by my boss and the other powers that be. There really aren't a whole lot of other similar programs around the country to compare us with either, and Duke is somewhat different in how it runs its graduate programs anyway. So there are a lot of questions that still remain about how best to implement an assistantship that also can serve as a training program. So, as a form of self assessment, here is briefly how I would design a basic, one-year digital humanities program in the abstract.

The model student I am thinking of for this thought experiment will be a Duke graduate student enrolled in a humanities PhD program, likely during their waning years in the program while dissertating, and with some experience in computer or web development that would make them a likely candidate for the position. The design is somewhat similar to my own experience, but much more heavily condensed and optimized for a single year.

### Semester 0: Summer Pre-Training Exploration

Duke doesn't usually have Humanities funding over the summer, but our student will likely already be on campus or available in some sense to do some summer training between Spring and Fall terms. This would be a good time to get some initial skills under their belt, if they don't have them already. I would recommend at least three skills, JavaScript, basic Terminal commands, and HTML/CSS.

JavaScript is pretty standard throughout the world, and with platforms like [Node.js][1] gaining in popularity, it is likely that it will become even more popular. So I would start with JavaScript as a programming language, maybe learn some of that over the course of the summer. It can also serve as a base for any other programming languages that one has to learn in the future. [Codecademy][2] is where I got some of these skills initially, but I find the emphasis on creating games (a ludic pedagogy) as a way of learning a little off-putting, as I would rather learn some real life utilities. I do like how they walk through most of the assignments, rather than just letting you sink or swim like in most online courses.

As for learning the terminal commands, I am wishing I learned all these things much sooner. I suppose that at some point one will be able to access a web platform interface without having to tackle any terminal time, but we are certainly not there yet. So learning basic terminal command would be ideal. I should mention that I got a good tutorial in both of these things from the [Coursera Startup Engineering MOOC][3] that was offered last year. It serves as an introduction to both of those things. The lecture notes can be found online.

As for HTML/CSS, I think this is a given. I find it hard to believe that a student would be a good candidate for the position without some experience in these languages. But it is always good to get a refresher and see what is new in the world. I know that learning about [Twitter Bootstrap][4] was an eye-opener for me, and I am sure there are some other packages worth knowing about.

There may be a few other things to work on over the summer, based on the student's particular interests. I almost put PHP on the list of things to learn, but I decided JavaScript might be a little better as a base language to start coding on, especially since [D3][5] might be an interesting package for a more data intensive person to learn. This can of course be modified based on the individual students skills.

It might also be helpful to browse some of the abstracts at some point from a recent DH201X conference, to see what people are doing these days in the DH world. Actually being able to attend [DH2013 in Lincoln][6] was great stroke of luck for me, both as a networking tool and as a learning experience. But just looking through the abstracts may spark someone's interest in one particular area or another. I know I learned more a lot more about what people are doing, both the very big and professional projects and the smaller, more feasible projects for someone just starting out. ([Here is the link for the DH2013 abstracts][7])

### Semester 1: Learning and Teaching Popular Tools for DH Research

So now that the student has a basic grasp of web development, it is time to throw them into to the deep end to learn some popular tools for DH research. These should probably include TEI, Omeka, Neatline, and WordPress at the very least. Knowing how to use them should also probably include how to set them up from scratch on a web server. I have found AWS very helpful in this regard, since it allows me to test run a large number of things for very cheap, free if I only have one instance running at a time. If a semester is 4 months long, this also gives the student an opportunity to master one a month.

There are also a number of other tools that can be useful to explore. Gephi is one of them, and perhaps some initial exploration in GIS would be helpful (something I am not all that comfortable with right now). Through the course of investigating these tools, the student should be encouraged to consider how the tools could be used in conjunction with their own research, and perhaps be invited to tinker around a little bit and see how they can integrate it. This is how I found out that I could integrate the Pleiades Data in Google Fusion tables, for instance.

This would be supplemented by whatever work is needed by the people in charge. A lot of my knowledge comes from training people in this or that, and that certainly was different depending on who was running a particular project. So, if a researcher needs help with some project involving mapping, that would be a good way to learn about whatever tool they are thinking about using.

### Semester 2: Integration into Research

In the second semester, I would encourage the student to develop their own project for their research. This likely would stem out of some the work that they did over the course of the first semester, with goal of answering some sort of research question derived from their own work. This would obviously differ depending on the student and what their program was. While they are working on this, it will be assumed that they will continue to support faculty and researchers so they can cement their understanding of what they learned the previous semester.

The scope of this project would necessarily have to be limited in order to be implementable by the end of the semester. The project I am working on, the [Ancient Sports Atlas][8], is probably a much bigger project than most would be, but I am working on it in a piecemeal fashion that will grow over time as new facets are added. Just getting it up and running will be a success for me. A more feasible project for someone in a one year program would perhaps be a large Omeka exhibit or a digital edition of a text.

All the meanwhile, I recommend keeping a blog running to store their findings, reviews, and experiments. This serves the purpose of tracking of the work the student is doing as well as serving as a portfolio of their work. The [Duke Certificate in College Teaching][9], by way of a parallel, asks students to assemble a website for their teaching materials and as an online home page when they searching for jobs, so this blog can serve that purpose as well. I have thought it might be a good idea to have a digital humanities graduate student blog at Duke where these students could contribute as well. Both the final project and this blog can serve as a form of assessment for outcomes.

* * *

So that's it for an outline of a program, which I think would be a good introduction to the topic for humanities students at Duke. There are certainly other models that may work better. [The Praxis Network][10], in which Duke is included, collects together schools that are looking for ways to educate graduate students in the Digital Humanities. I especially am intrigued by the model at UVA. Their [Praxis Program][11] brings together graduate students to work on a specific tool in a team learning project. I really like this idea, but it seems that it doesn't allow as much for the student to explore how it could influence their own research. But maybe not: I'd like to see how it works out in practice.

* * *

Image credit: [Tagxedo][12] of the [Duke Doing DH blog][13].

[1]: http://nodejs.org/ "Node.js"
[2]: http://www.codecademy.com/ "Codecademy"
[3]: https://www.coursera.org/course/startup "Coursera | Startup Engineering"
[4]: http://getbootstrap.com/ "Twitter Bootstrap"
[5]: http://d3js.org/ "D3.js"
[6]: http://dh2013.unl.edu/ "DH2013"
[7]: http://dh2013.unl.edu/abstracts/ "DH2013 Abstracts"
[8]: http://danielgriff.in/ancient-sports-atlas/ "Ancient Sports Atlas"
[9]: http://gradschool.duke.edu/prof_dev/cct/index.php "Certificate in College Teaching"
[10]: http://praxis-network.org/ "Praxis Network"
[11]: http://praxis.scholarslab.org/ "UVA Praxis Program"
[12]: http://www.tagxedo.com/ "Tagxedo"
[13]: http://sites.duke.edu/digital/training-events/past-events/doing-dh/ "Doing DH"
