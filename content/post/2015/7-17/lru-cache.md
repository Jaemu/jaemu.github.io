+++
date = "2015-07-17T12:57:25-04:00"
draft = true
title = "An Introduction to LRU Caches"
description = "I've been asked how to build one in several technical interviews, so I'm going to do it in every language I claim to know"
tags = ["algorithms", "programming", "interview-questions"]
+++


When building and subsequently scaling a website, caching is important.  At the scale of hundreds of thousands of users a day, having multiple layers of caching becomes critical.  


On the job, we use the in-memory data store [redis](http://redis.io/) heavily (someone did a talk about it once which you can find [here](http://blog.pivotal.io/pivotal/case-studies-2/8-ways-media-giant-viacom-uses-redis-to-serve-dynamic-video-at-scale)).  At a high level, we use it to support data coming into our sites so when we do things on the site that require lots of rapidly-changing content it's pretty important to bust cache while making sure the site stays up.  


I've also done two technical interviews in which I've been asked to implement a cache with a [least recently used](https://en.wikipedia.org/wiki/Page_replacement_algorithm#Least_recently_used) eviction policy.  In a nutshell, LRU is a policy of dropping the "least recently used" item from cache to make room for a most recently used item that does not exist in the cache.  


Question: Implement a cache with an LRU policy that has O(1) insertion, O(1) lookup, and O(1) removal.


#### Insertion


In order to achieve O(1) insertion, you will need to store items themselves in a hash map or a linked list.  At first, it seems obvious that this list should be ordered so a queue might suffice.  

#### Lookup


Since lookup needs to be O(1), we need to store items in a data structure that gives us an O(1) key-value mapping.  However, we also need to be aware of the order of insertion *and* order of being looked at, which we lose if we get rid of the queue entirely.


#### Removal