---
layout: post
title:  Suffix Trees
author: Rahul Pandey
date:   2017-02-03
categories: suffix trees trie
---

The suffix tree is probably the most powerful and versatile data structure for string processing that's ever been invented. A suffix tree is a compressed trie containing all the suffixes of a given text as keys, and positions in the text as their values. We'll unpack what that means, but suffix trees enable particularly fast implementations of many important string operations, such as: 

- Find a substring in string `S`, even if we want to allow a certain number of mistakes in the substring. 
- Locating matches for a regular expression pattern within a string. 
- A linear time solution for the longest common substring problem. 

The tradeoff for this power is that the storage for a string's suffix tree requires several multiples more space than storing the string itself. 

Suffix trees are used heavily in bioinformatics, where we often want to search for patterns in DNA or protein sequences (which can be viewed as strings of characters). We can also use suffix trees for data compression, where we can easily find repeated data. Suffix trees are ideal anytime we can afford to spend a lot of time pre-processing the text, but we want to answer queries about the text very quickly. 

Contents
===========

- [Suffix Tries](#suffix-tries)
- [Suffix Trees](#suffix-trees)

## Suffix Tries

Let's start by talking about a suffix trie. The suffix trie is the smallest tree such that: 

- each edge is labeled with a chaaracter in the alphabet
- each node has at most 1 outgoing edge labeled with c, for any c in the alphabet
- each suffix of the tree is "spelled out" along some path starting at the root of the tree. 

The downside of a suffix trie is that the amount of space required to store the trie grows quadractically with respect to the length of the input string. So for even relatively small strings, say 500 characters, the number of nodes required in the trie could be more than 100K. This leads us to a suffix tree, which compresses the tree. 

## Suffix Trees

The suffix tree for a string `S` of length `n` is defined as a tree such that: 

- The tree has exactly n leaves numbered from 1 to n.
- Except for the root, every internal node has at least two children.
- Each edge is labeled with a non-empty substring of S.
- No two edges starting out of a node can have string-labels beginning with the same character.
- The string obtained by concatenating all the string-labels found on the path from the root to leaf i spells out suffix S[i..n], for i from 1 to n.

TODO: explanation of construction and runtime, examples

### Sources

- [Stanford CS166 Lecture (April 7)](http://web.stanford.edu/class/archive/cs/cs166/cs166.1166/)
- [Wikipedia link](https://en.wikipedia.org/wiki/Suffix_tree)
- [Ben Langmead's Youtube walkthrough](https://www.youtube.com/watch?v=hLsrPsFHPcQ)