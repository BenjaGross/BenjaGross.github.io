---
layout: post
title: "About Trie-ing"
date: 2016-02-19 10:57:20 -0500
comments: true
categories: 
---

I recently had the chance to work on a problem involving pattern matching. One of my favorite aspects of programming is solving puzzles and I quite enjoyed working on this problem. 

After finding my first solution for matching strings with a nested loop I figured that there must be a better matching algorithm. After researching a bit I came accross the [Trie](https://en.wikipedia.org/wiki/Trie) data structure. A Trie can look very similar to a Binary Search Tree as seen below (Trie: left Binary Tree: rigth).

![Trie](../images/trie.png) ![Binary Tree](../images/binary_tree.png)

Although Tries use a tree like structure they are different from a normal trie for several reasons.

+ Each descendent of a node all share a common prefix like "i", "in", and "inn" in the above example. I would also guess that this is why they are allso called "Prefix Trees"
+ No node in the tree stores any key associated with that particular node, instead the location in the tree defines what key each node is associated with 
