+++
date = "2015-07-17T12:57:25-04:00"
draft = true
title = "An Introduction to LRU Caches"
description = "I've been asked how to build one in several technical interviews, so I'm going to do it in every language I claim to know"
tags = ["algorithms", "programming", "interview-questions"]
+++


On the job, we use redis pretty heavily (someone did a talk about it once which you can find [here](http://blog.pivotal.io/pivotal/case-studies-2/8-ways-media-giant-viacom-uses-redis-to-serve-dynamic-video-at-scale)).  At a high level, we use it to support data coming into our sites so when we do things that require lots of rapidly-changing content it's pretty important to bust cache while making sure the site stays up.  


I've also done two technical interviews in which I've been asked to implement a cache with a [least recently used](https://en.wikipedia.org/wiki/Page_replacement_algorithm#Least_recently_used) eviction policy.  