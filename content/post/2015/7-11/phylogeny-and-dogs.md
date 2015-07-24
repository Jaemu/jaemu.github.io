+++
date = "2015-07-11T12:57:25-04:00"
draft = false
title = "Evolutionary tree reconstruction and the phylogeny of the domestic dog pt. 1: Phylogenetic trees and edit distance"
description = "What I learned from computational biology and being a dog owner"
tags = ["algorithms", "biology"]
+++

I love life sciences, dogs, and algorithms so when I found [this paper](http://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1004016) describing recent changes to the hypothesized phylogeny of modern dog I got giddy.  

Dogs are incredibly fascinating, regardless of your opinion on breed standards and dog shows.  They are the only successfully domesticated carnivore, they have the ability to understand our social communication mechanics and language, they engage in mutually beneficial social cooperation with us and we are not absolutely certain of why or when it happened.


The truth is there is a lot we don't understand about *how* man domesticated dogs or if man was even the catalyst for domestication.  For a long time, the prevailing theory was that dogs had evolved from wolves that were attracted to human agricultural settlements by trash.  This isn't completely unrealistic because we know wolves are [scavengers](https://en.wikipedia.org/wiki/Dog_anatomy#Physical_characteristics) and that dogs have maintained some of these behaviors (any dog owner can tell you about their temporary land shark that appears around the table at mealtime).  


What is so awesome about the findings from this study is that it demonstrates a working model of phylogeny that satisfies the current state of genetic similarity that shows dogs not as a derivative of wolf but as a [sibling that evolved separately from a common ancestor](http://mediarelations.cornell.edu/2014/01/16/dogs-and-wolves-diverged-from-common-ancestor/).  ![Neighbor-joining tree and admixture signatures from ABBA/BABA tests.  Source: Genome Sequencing Highlights the Dynamic Early History of Dogs
Freedman AH, Gronau I, Schweizer RM, Ortega-Del Vecchyo D, Han E, et al. (2014) Genome Sequencing Highlights the Dynamic Early History of Dogs. PLoS Genet 10(1): e1004016. doi: 10.1371/journal.pgen.1004016](/img/journal.pgen.1004016.g004.png)

#### Phylogenetic trees: introduction and construction by phenotype

The diagram in this picture is called a phylogenetic tree, a data structure that maintains the evolutionary relationship between child nodes.  In this representation, children on the same depth of the tree are "siblings" and their "common ancestor" is the node that corresponds to their closest common parent node.  So in this tree, BOX (boxer) and BSJ (basenji) are more closely related because they have a common ancestor that lived more recently than that of say, BOX and DNG (dingo).  Typically, the height of the tree correlates to time but I'll explain the motivation for using base pair differences.


Before genetic sequencing, the "distance" between sibling nodes and the placement of intermediary branching events was guessed at by similarity of observable traits, or [phenotype](https://en.wikipedia.org/wiki/Phenetics).  


There are several problems with using the phenetic approach, most notably the existence of similar traits in unrelated species that have different evolutionary paths.  This could have occurred in isolated populations that were subject to the same selective pressures and not indicative of parent or sibling relationships in evolution.  


Enter genomics and [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance).

#### Phylogenetic trees: construction by edit distance


With cheaper, faster genetic alignment and sequencing possible, we can now infer evolutionary relationships between species based on the "distance" of their DNA.  


It's not as simple as determining the edit distance between two long strings -- identifying known gene regions and the migration, duplication and deletion of genes as well as rate of population growth and appearance of mutations across species [complicates this a bit](https://en.wikipedia.org/wiki/Genetic_distance).  


I will go into more detail of the algorithms used to reconstruct phylogenetic trees from genetic distance, but before you can construct a phylogenetic tree you need the data and a metric with which to measure the relative evolutionary relationships between species.


To determine the degree of "similarity" between two different species using DNA, it is necessary to sequence the current members of the species to compare and calculate their genetic difference.  In this case, siblings with a more recent common ancestor would be more genetically similar than species with an ancestor that last spread its genetic data thousands of years ago.


Which brings us back to the base pair representation of time.  Using base pair differences to determine similarity between species, a greater genetic distance directly correlates with a more distant common ancestor.  The program used to determine genetic distance in the study is [described here](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3245873/).  In the diagram above, "substitutions per 10,000 base pairs" is the number of substitutions that must be made to one species' genetic sequence to transform it into the other (in this diagram, dashed lines represent [genetic admixture](https://en.wikipedia.org/wiki/Genetic_admixture)).  


When you combine this data with known data of populations, migrations, and approximate times of divergence between two species, we can begin to pinpoint the time at which these genetic events occurred based on the relation between known divergence events and another species' genetic distance from that event.  


#### New findings in the genetic lineage of domestic dogs


With this model of the phylogeny of modern dog and its relation to wolves, the study suggests that the closest living relative that produced both species occurred between 9,000 and 34,000 years ago.  If this tree is the correct model of speciation for all of the members of the canine family sampled, it makes the previously mentioned theory of dog domestication much less probable.  Other studies of dog-like fossils support the model of evolution that shows dog as having begun its divergence from its ancestor [15,000 to 34,000 years ago](http://www.researchgate.net/publication/258529165_Complete_Mitochondrial_Genomes_of_Ancient_Canids_Suggest_a_European_Origin_of_Domestic_Dogs).  


15,000 to 34,000 years ago is before the age of agriculture, and recorded history begins.  If this is true, the domestication of dogs began during the age of men as a hunter/gatherer society.  So what does that mean for the role of humans in the domestication of dogs if the dog genome began selecting for domesticated traits before what we consider modern man existed?


I was reminded of [this article](http://www.nytimes.com/2012/05/29/science/dogs-and-humans-speculation-and-science.html), which back in 2012 discussed the "speculative science" of the dog genome because there was no credible evidence to indicate that dogs played any role in modern human's dominance over neanderthal.  While this study doesn't provide conclusive evidence that dog and man co-evolved, it does support the idea that dogs and humans at the very least may have engaged in mutually beneficial social cooperation long before humans discovered agriculture and demonstrates pretty clearly that the ancestor to the modern dog existed during the time of neanderthals.  


#### Final thoughts

I don't pretend to be an evolutionary biologist so don't take my word on the quality of the science mentioned in any of the papers or articles that I cited.  I sort of fell down a rabbit hole when trying to find an article to support my dad's assertion that he once heard on the Discovery channel that humans evolved a large visible sclera due to evolutionary advantages and the cooperative eye [cooperative eye hypothesis](https://en.wikipedia.org/wiki/Cooperative_eye_hypothesis) and that dogs also engage in social cooperation via eye communication.  Much to my surprise, my dad wasn't totally contradicted by modern science (not a first, but a rarity).


While it is true that dogs can and do signal emotions through use of visible sclera (search "whale eye" and you will get lots of images of scared or anxious dogs), there is no evidence that they engage in social cooperation with humans using eye contact.  The jury is still out on whether or not eye contact with humans makes dogs feel a stronger social bond or feel threatened.  


-----

I always try very hard to separate observable fact from gut feeling, and that's almost impossibly hard to do when it comes to observing something you're very attached to.  


Is it wishful thinking on my part to imagine primitive man and the ancestor of dog living and working together as a cooperative unit?  What advantage did we give ancestral dog in return that gave it such a competitive advantage over its peers?  Was it a larger shared predator or environmental pressures or something else entirely?  Did we rely on dogs to be our literal eyes and ears and over time this caused our senses to become much duller?


I look at my dog now snoring under my chair, curled up on an old sweater of mine, nose-first in my shoe watching her take deep whiffs of my scent which is kind of gross but also really sweet in a way.  She has lots of very strong instinctual behaviors like digging ditches to get to cool dirt, herding various family members (most of the time without us dumb monkeys figuring out that she's sleeping soundly only because all of us are sitting on the couch), what is known as the ["border collie stare"](https://www.google.com/search?q=border+collie+stare&safe=off&espv=2&biw=1394&bih=821&tbm=isch&tbo=u&source=univ&sa=X&ved=0CB0QsARqFQoTCKjJm9OB48YCFc06kgodBdEP-A&dpr=0.9) when she's waiting for me to throw her frisbee, etc.  


And yet even though man has molded domestic dog evolution to use its instincts to serve his needs like herding livestock and retrieving game, we shouldn't give ourselves too much credit.  

![Meryl on selection day](/img/puppe.jpg)


They chose us too.


-----

Further Reading: 

- ["Taming the Wild"](http://ngm.nationalgeographic.com/2011/03/taming-wild-animals/ratliff-text/2), an article from Nat Geo describing the efforts to study the genetic origins of domesticated behaviors.  Of particular interest is the assertion made that domestication is a fluid rather than linear unidirectional process.
- ["Urinary oxytocin and social bonding in related and unrelated wild chimpanzees"](http://rspb.royalsocietypublishing.org/content/280/1755/20122765), a paper on the chemical changes in chimpanzees the engage in social cooperation
- ["How Much Is That in Dog Years? The Advent of Canine Population Genomics"](http://journals.plos.org/plosgenetics/article?id=info:doi/10.1371/journal.pgen.1004093), paper describing the history of the study of dog phylogeny.
