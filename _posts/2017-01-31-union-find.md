---
layout: post
title:  Union Find
author: Rahul Pandey
date:   2017-01-31
categories: union find disjoint sets
---

The Union Find (also called Disjoint Sets) data structure is a data structure to merge disjoint sets efficiently. The context is that you have a large number of items partioned into several smaller sets. All items are part of exactly 1 set, and there is no overlap between any 2 sets. The supported operations are:

- Find: which set does an item belong to?
- Union: merge 2 sets together

Two implementations of the data structures optimize for the efficiency of Find vs Union. With QuickFind, find is constant time, while union is linear time. The more interesting implemenation is QuickUnion, in which union is constant time, while find is log(number unions) done. The usual way to implement this is using a (non-binary) tree. Each set is a separate tree, and the set is identified by the root (“canonical node”). To union two sets, the smaller tree becomes a sub-tree of the other, which makes it an O(1) operation.

Find can be done in O(log u) time, where u is the number of unions. To find an item in this implementation, represent the tree as an array where positive numbers indicate the parent node, and negative numbers represent the size of the set rooted at that node. (Path compression is an optimization.) This grows proportional to the inverse Ackermann function, which is practically a constant ~3. However, that is a topic for another blog post. 

TODO: real world examples

### Sources

- [Disjoint Sets](https://www.youtube.com/watch?v=gcmjC-OcWpI)
