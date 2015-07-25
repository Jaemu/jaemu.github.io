+++
date = "2015-07-17T12:57:25-04:00"
draft = true
title = "An Introduction to LRU Caches and Implementation"
description = "I've been asked how to build one in several technical interviews, so I'm going to do it in every language I claim to know"
tags = ["algorithms", "programming", "interview-questions"]
+++


When building and subsequently scaling a website, caching is important.  At the scale of hundreds of thousands of users a day and beyond, having multiple layers of caching becomes critical.  


On the job, we use the in-memory data store [redis](http://redis.io/) heavily (someone did a talk about it once which you can find [here](http://blog.pivotal.io/pivotal/case-studies-2/8-ways-media-giant-viacom-uses-redis-to-serve-dynamic-video-at-scale)).  At a high level, we use it to support the data feeds coming into our "frackend" to further reduce the amount of calls we're making to the services layer and ultimately the database.    


Besides working with redis, I've also done two technical interviews in which I've been asked to implement a cache with a [least recently used](https://en.wikipedia.org/wiki/Page_replacement_algorithm#Least_recently_used) eviction policy.  In a nutshell, LRU is a policy of dropping the "least recently used" item from cache to make room for a most recently used item that does not exist in the cache.  


Question: Implement a cache with an LRU policy that has O(1) insertion, O(1) lookup, and O(1) removal.


##### Insertion


In order to achieve O(1) insertion, you will need to store items themselves in a hash map or a linked list.  At first, it seems obvious that this list should be ordered so a queue might suffice.  

##### Lookup


Since lookup needs to be O(1), we need to store items in a data structure that gives us an O(1) key-value mapping.  However, we also need to be aware of the order of insertion *and* order of being looked at, which we lose if we get rid of the queue entirely.


#####  Removal

If removal is O(1), that means we need to be able to remove key-value pairs in O(1) time.  A linked list will not suffice here either since list traversal is O(n).


So to recap, ideally we need a hash map to do plain O(1) insertion, lookup, and removal but we also need a queue to keep track of order of insertion.  Is there a data structure that does both?

- Java: Yes.  [LinkedHashMap](http://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html) is a thing, but you should still prove that you can implement the cache with a linked list and a hash map and that you understand how all of these data structures work.  
- Python/C: No, but we can fake it with a dictionary of objects (structs, etc.) that store references to nodes in a linked list.
- Javascript: Heh.  Good one, guys.

-----


#### Solution Logic


Before I dive straight into code, it would be helpful to figure out what the code should do first.  

##### Insertion: 
There are two cases for insertion.

1. The cache has available space.
2. The cache is full and an item must be deleted before insertion of the new element.


So really, insertion actually has a hard dependency on lookup (does this key exist?) and removal (If there is no space, make space)so we should implement those first.  

##### Removal: 
There are technically two cases for removal, but one is "is the cache empty" and if we maintain a size parameter on insert and removal this is trivial to check for.  

In our cache, removal is a sub-operation of insert.  But retrieval is also a sub-operation of removal since we need to find the item to delete it.  

##### Retrieval: 