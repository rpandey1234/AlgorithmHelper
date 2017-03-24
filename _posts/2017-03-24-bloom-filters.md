---
layout: post
title:  Bloom filters
author: Rahul Pandey
date:   2017-03-24
categories: bloom filter
---

Bloom filters are a probabilistic data structure which tell us that an element is *definitely not* in a set, or it *may be* in a set. Bloom filters are designed to give you an answer to (potential) element existence very quickly and memory-effeciently. When constructing a Bloom filter, elements can be added to the set but not removed. The more elements added to the set, the larger probability of false positives. 

Said another way, Bloom filters allow false postives but not false negatives. The benefit of allowing this inexactness is that we can drastically reduce the memory requirement that a "conventional" error-free hashing function would require. 

## Implementation details

Bloom filters are backed by a bit vector. To add an element, we simply hash it a few times and set the values in the bit vector at the index of those hashes to true (or 1 if that is your "on" state). For example, suppose we are consructing a Bloom filter over a set of strings. We can use 2 different hash functions which, given a string, output a number from 0 to 15 (we have control over the output ranges of the hash functions). The result of hashing string `S` using the 2 functions gives us 9 and 14, which we set to true in the bit (or boolean) vector. Now, if we want to test the existence of string `S2` in our set, we will hash it using the same 2 hash functions. If the results are not 9 *and* 14, we know without a doubt that `S2` is not in the set. On the other hand, if the hash functions do output the right values, all we can say is that `S2` may be in the set. 

The hash functions used in a Bloom filter should be independent, uniformly distrubuted, and fast. 

The false positve rate is approximately (1 - e<sup>-kn/m</sup>)<sup>k</sup>, where we use *k* hashes, *m* bits in the filter, and *n* elements in the Bloom filter already. This makes sense in the extremes: 

- if we have infinite elements (*n = &infin;*), the false positive rate goes to 1 since the exponent on *e* will be a large negative number. 
- if we have infinite bits in the filter (*m = &infin;*), the false positve rate goes to 0 since the exponent on *e* will be 0. 

## Use cases

Bloom filters are used extensively when trying to avoid an expensive operation such as for disk reads. For example, Google BigTable, Apache HBase, and Postgresql all use Bloom filters to reduce the disk lookups for non-existent rows/columns, resulting in a much better query operation. 

Another use case is to filter out stories that users have seen before. If you have many more stories to serve users than they've actually seen, you are likely fine with the inexactness of Bloom filters as long as you can guarantee that all their stories are fresh. 

### Sources

- [Bloom filters wiki](https://en.wikipedia.org/wiki/Bloom_filter)
- [Bloom filters tutorial](https://llimllib.github.io/bloomfilter-tutorial/)