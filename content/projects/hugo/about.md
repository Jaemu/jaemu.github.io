+++
date = "2015-04-14T22:17:00+02:00"
draft = false
title = "jillmunson.com + hugo"
description = "I made a website! (after lots of false starts and with some help)"
tags = ["personal", "projects", "front-end"]
+++

One of the biggest weaknesses I have is putting pen to paper, finger to keyboard, etc. without getting overwhelmed by trying to organize something that isn't even there.  Even in writing, my comfort zone was always editing and not content creation (I blame this background for my preference for hunting bugs and re-organizing code than architecting an application from scratch).  I think as you get more knowledge without constantly exercising that knowledge with repeated exercise this becomes incredibly common.  


As a primarily front-end developer, this overthinking paralysis has stricken me every time I've tried making a personal site.  I've experimented with different blog engines, tried rolling my own, even tried just hard-coding static html pages as a thought experiment to try and identify the different pain points that were causing me to throw in the towel every time.  


All in all, I learned several things about myself and side projects in general:


#### Lesson 0 - You *might* not need (javascript|insert favorite language here) 

When I first started thinking about what I needed, I started looking into [Ghost](https://github.com/tryghost/Ghost), which is a full blog engine with a ton of features out of the box, 3rd-party plugins, etc.  Ghost is great and from my little time with it, seems like the kind of thing that could really fit the use case of needing a blog engine.


Once I started getting set up with it though, I realized that for what I wanted (a personal site to host blog posts, images, and project pages) I didn't really *need* a full-fledged backend and thus didn't really need a server to serve content.  What I really wanted was something that generated static content and could be served from something like AWS S3 or Github.


This was an important realization for me because one of my major problems was just identifying what it was that I wanted to build.  Even though I could run a self-hosted instance of Ghost, that required a lot of additional configuration and setup that while not *difficult* per se was an additional headache (not even going into my gun-shyness with securing a public web server after a fiasco with an EC2 instance that I was running for a network security project of all things).


Even though I was completely comfortable with nodejs and have been working on an incredibly dyanmic single-page application entirely in javascript, the things that nick.com need to do vs. what I wanted my site to do were wildly different in scope and complexity.  


I learned not to overcomplicate things just for the sake of "learning" and to make the right thing for the right task.  


#### Lesson 1: While not always the right way to go, doing something the hard way first teaches you a lot about what you're trying to build



I wish I could say that I learned my lesson and continued on without making things overly complex.


Instead, I thought "well I can write my *own* static site generator for nodejs!  With recursion and partials and more!"


As an experiment to figure out what features I would need to do so, I tried hand-rolling my own site.  In hindsight, this is like milking a cow to figure out how to make an ice cream machine but hindsight.


At the very least, I identified some pain points:

-  Re-usable HTML templates and partials are not just a nice to have, they are a necessity.  Otherwise, woe is the developer trying to keep an ever-growing number of static files consistently pointing to the right file references, using the same class names for elements across pages, etc.
-  Writing a static HTML generator is foolhardy for several reasons, least of which is the existence of Markdown compilers.  I realized one of the biggest advantages of using Markdown is that not only is there a common language that compiles to the same elements, but it gives you the freedom to use inline HTML when absolutely necessary.  
-  Writing responsive CSS from scratch is a pain.  I don't care how long you have been doing front-end development, writing clean, maintainable, working CSS without Less/Stylus/Sass and a framework is an exercise in frustration.  

There were probably many more, but that was about as far as I got before having a conversation with my friend [Rob](http://www.robxu9.com/) about what I was trying to do.  He suggested [hugo](http://gohugo.io/).


#### Lesson 2: Done is better than perfect, or why you shouldn't give up on something the first time it doesn't work the way you expect it to.

Getting started with hugo was extremely easy.  
````
brew install hugo
````

Figuring out what to do from there was not as easy.  Hugo expects to create a new folder within the current directory of 
````
hugo new site (sitename)
````
and if that folder already exists, throws an error.  If it were me, I would have created a new site within that folder or at least created an option to pass a flag that indicates that this is what I want to do, especially when migrating content from an existing github repo that has the strict requirement of having the name (github_name).github.io.  

Once I got past that, I stumbled on the configuration bit.  Hugo expects a config.(toml|yaml|json) file, but not all of the params are documented.  This becomes a problem when trying to add data to partials because *some* data is global in scope but others like custom params are under the .Site.Params namespace.  [This is all that I was able to find](http://gohugo.io/overview/configuration/), and while most of the params are documented some are given no explanation.  

There is also some reduntant functionality.  In the config file, you can specify which theme you want to use but you can also compile your project with 
````
hugo -t (theme_name)
````
which is fine, but again not super well documented as to why you would want to use one and not the other.

---

My point is, it's ok to struggle with a tool out of the box.  The beauty of open source is that if something doesn't work the way you intend it to, look at the source and if you think it's wrong then change it.  

#### Lesson 3: Configuration is important.  Flexible configuration is a god-send.


One of my favorite parts about hugo is the configurable folder structure.  Currently I'm publishing to the project root directory (bad), but in the config file I can specify all of the directories I want to use and where I want hugo to publish those files.  I ran into this issue with Octopress awhile ago and found the "use this file structure" heavyhandedness really inflexible so this feature was a nice surprise.  

#### Lesson 4: There are hackfixes in *every* project.  

When I first tried to deploy the site to github, all of my static file references were 404ing even though they were in the right directories.  I realized after a tiny bit of digging that when you run 
````
hugo server
````
it replaces the baseurl param with "localhost:1313" and puts this all over your site so that static file references work locally.  This is extremely heavyhanded (what if I don't have localhost in /etc/hosts and I don't have permissions to change that file?  Hypothetical.) for a few reasons.  I guess you could get around this by not using absolute paths to reference your static content, but that seems rather [opinionated](http://stackoverflow.com/questions/2005079/absolute-vs-relative-urls).  

#### Conclusions

I ran into a few other issues (static files not being copied over into my publish dir frrom my theme folder, customizing the theme, etc.) but all in all I found hugo easy to work with if a bit under-documented.  Most of its power comes from its tiny footprint, speed, and flexible configuration.  I don't mind the hard dependency on Go, but I can see that being a drawback for some (although I find Go configuration and package management much easier to manage than, say, python but I could be biased because I am using a mac with a default python installed and having to remember where python vs python2.7 vs python3 look for their packages is frustrating at best).  


This is one person's experience with one tool, so please take any opinions here with a grain of salt.  However, the lessons learned here apply to any project.  


