+++
comments = true
date = "2015-07-25T14:47:49-04:00"
draft = false
image = ""
share = true
tags = ["python", "app-engine", "apis"]
title = "Haiku.py"
description = "Generating haikus using flask and nltk on google app engine"
+++

Github: [haiku.py](https://github.com/Jaemu/haiku.py)


Demos: 

- Haiku generator:  [/haiku.json](http://haiku.jillmunson.com/haiku.json)

- Syllable counter:  [/syllable](http://haiku.jillmunson.com/syllable/flower)
 
- Insult generator (Disclaimer: may sometimes produce compliments): [/insult](http://haiku.jillmunson.com/insult)


A work in progress JSON API haiku-as-a-service.  Built with Flask on top of the provided sample application from CP:100, deployed on [google app engine](https://cloud.google.com/appengine/docs)


------

#### v.01 - counting syllables with nltk and CMU pronouncing dictionary


This all started as a demo app for google app engine that I was tweaking as part of [CP:100](https://cloud.google.com/training/), an introduction to Google Cloud Platform that was organized by [GDG-NYC](http://www.meetup.com/NYC-GDG/) (side note: they are awesome and run some really awesome meetups so if you are in the area, go!).


At my day job, we use Hipchat for internal communications.  My original end goal was to build an API that would produce haikus from a user's chat history.  Since I don't have an API key for Hipchat access, I thought I would start on the whole counting syllables thing.  


Syllable counting is hard.  In theory, I could have started by limiting the dictionary and hard-coding text inputs but that sounded tedious and the whole goal of this project was to entertain myself so creating shit work would kind of defeat the purpose.  


Enter [nltk](http://nltk.org).  Nltk is a natural language toolkit for Python that has lots of [awesome modules](http://www.nltk.org/py-modindex.html) and [a companion book with examples](http://www.nltk.org/book) to get you started.  


Unfortuantely, nltk does not have a syllable dictionary.  However, I found this nifty [code snippet](https://groups.google.com/forum/#!msg/nltk-users/mCOh_u7V8_I/HsBNcLYM54EJ) from the nltk-users mailing list which was cited in this [writeup](http://runningwithdata.com/post/3576752158/w) which is something closer to what I ended up using:

````python
from curses.ascii import isdigit 
from nltk.corpus import cmudict 

d = cmudict.dict() # get the CMU Pronouncing Dict

def nsyl(word): 
    """return the max syllable count in the case of multiple pronunciations"""
    return max([len([y for y in x if isdigit(y[-1])]) for x in d[word.lower()]])
````

What this list comprehension is doing is taking the entry in the [CMU pronuncing dictionary](http://www.speech.cs.cmu.edu/cgi-bin/cmudict) and counting the number of pronunciation entries for a given word (using max to always return greatest number of syllables in cases of multiple correct yet discrete pronunciations).


