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

Link: [haiku.jillmunson.com](http://haiku.jillmunson.com)


Github: [haiku.py](https://github.com/Jaemu/haiku.py)


Demos: 

- Haiku generator:  [/haiku.json](http://haiku.jillmunson.com/haiku.json)

- Syllable counter:  [/syllable](http://haiku.jillmunson.com/syllable/flower)
 
- Insult generator (Disclaimer: may sometimes produce compliments): [/insult](http://haiku.jillmunson.com/insult)


A work in progress JSON API haiku-as-a-service.  Built with Flask on top of the provided sample application from CP:100, deployed on [google app engine](https://cloud.google.com/appengine/docs)


------

#### v.01 - Counting syllables with nltk and CMU pronouncing dictionary


This all started as a demo app for google app engine that I was tweaking as part of [CP:100](https://cloud.google.com/training/), an introduction to Google Cloud Platform that was organized by [GDG-NYC](http://www.meetup.com/NYC-GDG/) (side note: they are awesome and run some really awesome meetups so if you are in the area, go!).


At my day job, we use Hipchat for internal communications.  My original end goal was to build an API that would produce haikus from a user's chat history.  Since I don't have an API key for Hipchat access, I thought I would start on the whole counting syllables and making haikus that didn't totally suck thing.  


Accurate syllable counting is [hard](https://en.wikipedia.org/wiki/Syllabification).  In theory, I could have started by limiting the dictionary and hard-coding text inputs with syllable counts and part-of-speech tags but that sounded tedious and the whole goal of this project was to entertain myself so creating shit work would kind of defeat the purpose, although [it's been done](http://www.everypoet.com/haiku/chunk1.js).  


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

so the first version of the app literally just returned estimated syllable counts for a given input:

````python
import string
import nltk
from nltk.corpus import cmudict

class Haiku:

def __init__(self):
        self.cmudict = cmudict.dict()

def countSyllables(self, query='hello'):
        result = {}
        words = query.strip().split(',')
        for word in words:
            try:
                result[word] = max([len([y for y in x if y[-1] in string.digits]) for x in self.cmudict[word.lower()]])
            except:
                result[word] = 'Error - ' + word + ' is not a recognized word'
        return result
````

#### v.02 - Grammar tagging with Penn Treebank part of speech tags and nltk word tokenizer

So once I had a syllable counter, it would be trivially easy to find random words to construct a haiku that satisfies the 5-7-5 syllable structure.  I didn't want to settle -- I wanted haikus that, while maybe not semantically sensical, at least followed some semblance of grammatically correct English.

In order to be able to do this, I needed to be able to tag every word in the CMU dictionary with its part of speech.  The nltk textbook has a [whole chapter](http://www.nltk.org/book/ch05.html) dedicated to word tagging and tokenization.

For my purposes, I wanted to preserve the generated syllable count and the part of speech.  In order to do that, I needed to create a new dictionary.  Generating this dictionary on application load was insanely expensive, so I made a smaller script just to generate the dictionary and instead would load a pickle.

To test the dictionary, I created a new endpoint that would accept a word as input and return a list of other words that have the same part of speech and syllable count.

````python
import random
import string
import nltk
import pickle
from nltk.corpus import cmudict
from nltk import word_tokenize

class Haiku:


    def __init__(self):
        self.cmudict = cmudict.dict()
        self.words = self.cmudict.keys()
        self.tagged_words = pickle.load(open('tagged_words_syl.p', "rb"))

    def _syllableHelper(self, query='hello'):
	    return max([len([y for y in x if y[-1] in string.digits]) for x in self.cmudict[query.lower()]])


Using this pattern I generated haikus like:



    def getSimilarWords(self, word="hello"):
        wordData = nltk.pos_tag(word_tokenize(word))
        category = wordData[0][1]
        syllable = self._syllableHelper(word)
        similarWords = [x for x in self.tagged_words if x[1] == category and x[2] == syllable]
        return {
            word: similarWords,
            'category': category
        }


````

#### v.03 - Building pattern-based haikus

So once I had syllable counts and parts of speech, I needed to create a pattern for the haikus.  The pattern would contain three arrays, each containing either: 

- A (part of speech, syllable) tuple or
- A list of connecting phrases, modifiers, etc. 

The pattern I came up with after some trial and error was:

<p style="text-align: center; font-size: .75em;">
[plural noun] [present-tense verb]<br \>
[present-tense verb] ["in the" | "like some" | "without my" | "like the" | "of the"] [plural noun]  <br \>
[present-tense verb] [adjective] <br \>
</p>

So to use this to generate haikus, I would iterate over the pattern and either replace a tuple with a similar word or choose a random entry given a list of words.  I thought this might break down if I start accepting user-provided patterns, but I wasn't there yet and honestly was just curious what kind of output I would get.

````python

import random
import string
import nltk
import pickle
from nltk.corpus import cmudict
from nltk import word_tokenize

class Haiku:


	def __init__(self):
		self.cmudict = cmudict.dict()
		self.words = self.cmudict.keys()
		self.tagged_words = pickle.load(open('tagged_words_syl.p', "rb"))
		self.pattern = [[
				("NNS", 2),
				("VBG", 3)
			],[
				("VBG", 3),
				["in the", "with some", "like some", "without my", "like the", "of the"],
				("NNS", 2)
			],[
				("VBG", 2),
				("RB", 3)
			]
		]
		self.grammar = [
			"CC",
			"CD",
			"DT",
			"EX",
			"FW",
			"IN",
			"JJ",
			"JJR",
			"JJS",
			"LS",
			"MD",
			"NN",
			"NNS",
			"NNP",
			"NNPS",
			"PDT",
			"POS",
			"PRP",
			"PRP$",
			"RB",
			"RBR",
			"RBS",
			"RP",
			"SYM",
			"TO",
			"UH",
			"VB",
			"VBD",
			"VBG",
			"VBN",
			"VBP",
			"VBZ",
			"WDT",
			"WP",
			"WP$",
		]

	def _syllableHelper(self, query='hello'):
		return max([len([y for y in x if y[-1] in string.digits]) for x in self.cmudict[query.lower()]])	

	def getSimilarWords(self, word="hello"):
		wordData = nltk.pos_tag(word_tokenize(word))
		category = wordData[0][1]
		syllable = self._syllableHelper(word)
		similarWords = [x for x in self.tagged_words if x[1] == category and x[2] == syllable]
		return {
			word: similarWords,
			'category': category
		}

	def makeHaiku(self):
		haiku = {}
		for line in xrange(len(self.pattern)):
			haiku[line] = []
			for i in xrange(len(self.pattern[line])):
				if not (len(self.pattern[line][i]) > 2):
					currentPattern = self.pattern[line][i]
					category = currentPattern[0]
					syllable = currentPattern[1]
					similarWords = [x for x in self.tagged_words if x[1] == category and x[2] == syllable]
					haiku[line].append(random.choice(similarWords)[0])
				else:
					haiku[line].append(random.choice(self.pattern[line][i]))
		return {
			'haiku': haiku
		}
````

Using this pattern I generated haikus like:
<p style="text-align: center; font-size: .75em;">
schaefers beheading<br \>
reviving without my chloris<br \>
filling plaintively<br \>
</p>

and

<p style="text-align: center; font-size: .75em;">
bake-offs inflaming<br \>
taxiing in the promos<br \>
forcing gainfully<br \>
</p>


In general, the output reads like a silly but legit haiku.  