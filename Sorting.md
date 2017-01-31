# Sorting

Some algorithms (selection, bubble, heapsort) work by moving elements to their final position, one at a time. You sort an array of size N, put 1 item in place, and continue sorting an array of size N – 1 (heapsort is slightly different).

Some algorithms (insertion, quicksort, counting, radix) put items into a temporary position, close(r) to their final position. You rescan, moving items closer to the final position with each iteration.

One technique is to start with a “sorted list” of one element, and merge unsorted items into it, one at a time. 

Several O(n<sup>2</sup>) sorting algorithms exist, and while they may not be used much, it is valuable to have an understanding of how they work. 

### Bubble Sort [Best: O(n), Worst: O(n<sup>2</sup>)]
Starting on the left, compare adjacent items and keep “bubbling” the larger one to the right (it’s in its final place). Bubble sort the remaining N -1 items.

### Selection Sort [Best/Worst: O(n<sup>2</sup>)]
Scan all items and find the smallest. Swap it into position as the first item. Repeat the selection sort on the remaining N-1 items.

### Insertion Sort [Best: O(n), Worst: O(n<sup>2</sup>)]
Start with a sorted list of 1 element on the left, and N-1 unsorted items on the right. Take the first unsorted item (element #2) and insert it into the sorted list, moving elements as necessary. We now have a sorted list of size 2, and N -2 unsorted elements. Repeat for all elements.

### Quicksort [Best: O(n log n), Worst: O(n<sup>2</sup>)]
This is one of the most popular sorting methods. Here's the version using external memory: 

- Pick a “pivot” item
- Partition the other items by adding them to a “less than pivot” sublist, or “greater than pivot” sublist
- The pivot goes between the two lists
- Repeat the quicksort on the sublists, until you get to a sublist of size 1 (which is sorted).
- Combine the lists — the entire list will be sorted

### Heapsort [Best/Worst: O(n<sup>2</sup>)]
Add all items into a heap. Pop the largest item from the heap and insert it at the end (final position). Repeat for all items. Heapsort is just like selection sort, but with a better way to get the largest element. Instead of scanning all the items to find the max, it pulls it from a heap. Heaps have properties that allow heapsort to work in-place, without additional memory.

### Counting sort [Best/Worst: O(n)]
Assuming the data are integers, in a range of 0-k. Create an array of size K to keep track of how many items appear (3 items with value 0, 4 items with value 1, etc). Given this count, you can tell the position of an item — all the 1’s must come after the 0’s, of which there are 3. Therefore, the 1’s start at item #4. Thus, we can scan the items and insert them into their proper position. Radix sort has a similar idea but sorts integers one digit at a time. 

### Sorting algorithms in general
The fastest runtime for a comparison-based sorting algorithm is `O(n logn)`. This is provably true; you can convince yourself of this by noting that the complete decision tree for sorting `n` elements has height `Ω(n logn)`. There are at least `n!` leaves in the tree (one for each of the `n!` permutations of `n` elements). The worst case number of comparisons performed corresponds to the maximal height of the tree, `h`; a tree of height `h` has at most 2<sup>h</sup> leaves. From 2<sup>h</sup> ≥ `n!` => h ≥ `Ω(n logn)`.

# References
- [Sorting lower bound (Bowdoin class)](http://www.bowdoin.edu/~ltoma/teaching/cs231/fall07/Lectures/sortLB.pdf)
- [Sorting algorithms by BetterExplained](https://betterexplained.com/articles/sorting-algorithms/)
