+++
date = "2015-07-11T12:57:25-04:00"
draft = false
title = "Evolutionary tree reconstruction and the phylogeny of the domestic dog pt. 1: Phylogenic trees and edit distance"
description = "What I learned from computational biology and being a dog owner"
tags = ["algorithms", "biology"]
+++

I love life sciences, dogs, and algorithms so when I found [this paper](http://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1004016) describing recent changes to the hypothesized phylogeny of modern dog I got giddy.  

Dogs are incredibly fascinating, regardless of your opinion on breed standards and dog shows.  They are the only successfully domesticated carnivore, they have the ability to understand our social communication mechanics and language, they engage in mutually beneficial social cooperation with us and we are not absolutely certain of why or when it happened.


The truth is there is a lot we don't understand about *how* man domesticated dogs or if man was even the catalyst for domestication.  For a long time, the prevailing theory was that dogs had evolved from wolves that were attracted to human agricultural settlements by trash.  This isn't completely unrealistic because we know wolves are [scavengers](https://en.wikipedia.org/wiki/Dog_anatomy#Physical_characteristics) and that dogs have maintained some of these behaviors (any dog owner can tell you about their temporary land shark that appears around the table at mealtime).  


What is so awesome about the findings from this study is that it demonstrates a working model of phylogeny that satisfies the current state of genetic similarity that shows dogs not as a derivative of wolf but as a [sibling that evolved separately from a common ancestor](http://mediarelations.cornell.edu/2014/01/16/dogs-and-wolves-diverged-from-common-ancestor/).  ![Neighbor-joining tree and admixture signatures from ABBA/BABA tests.  Source: Genome Sequencing Highlights the Dynamic Early History of Dogs
Freedman AH, Gronau I, Schweizer RM, Ortega-Del Vecchyo D, Han E, et al. (2014) Genome Sequencing Highlights the Dynamic Early History of Dogs. PLoS Genet 10(1): e1004016. doi: 10.1371/journal.pgen.1004016](/img/journal.pgen.1004016.g004.png)

#### Phylogenetic trees: introduction and construction by phenotype

The diagram in this picture is called a phylogenetic tree, a data structure that maintains the evolutionary relationship between child nodes.  In this representation, children on the same depth of the tree are "siblings" and their "common ancestor" is the node that corresponds to their closest common parent node.  So in this tree, BOX (boxer) and BSJ (basenji) are more closely related because they have a common ancestor that lived more recently than that of say, BOX and DNG (dingo).  Typically, the height of the tree correllates to time but I'll explain the motivation for using base pair differences.


Before genetic sequencing, the "distance" between sibling nodes and the placement of intermediary branching events was guessed at by similarity of observable traits, or [phenotype](https://en.wikipedia.org/wiki/Phenetics).  


There are several problems with using the phenetic approach, most notably the existence of similar traits in unrelated species that have different evolutionary paths.  This could have occurred in isolated populations that were subject to the same selective pressures and not indicative of parent or sibling relationships in evolution.  


Enter genomics and [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance).

#### Phylogenetic trees: construction by edit distance


With cheaper, faster genetic alignment and sequencing possible, we can now infer evolutionary relationships between species based on the similarity of their DNA.  


It's not as simple as determining the edit distance between two long strings -- the location, migration, duplication and deletion of identical genes across species [complicates this a bit](https://en.wikipedia.org/wiki/Distance_matrices_in_phylogeny#Distance-matrix_methods).  