# Lists

We cover two types of lists in Java, though analogs exist in most other languages: `LinkedList<T>` and `ArrayList<T>` (note `T` indicates the type of element the list contains). For _most_ use cases, an `ArrayList` will better suit your needs. 

In a **linked list**, each element has a pointer to another element. A singly linked list is one in which pointers only exist in the forward direction, while each element in a doubly linked list has pointers to the next and previous element. A doubly linked lists makes it possible to traverse a list efficiently in both directions, with the tradeoff being the memory overhead of storing an additional pointer (generally 4 bytes) for each element. 

An **arraylist** is backed by an array, which is a contiguous chunk of memory. Each element of the list is assumed to have the same size, which makes random read access possible in constant time. 

                                 | `ArrayList` | `LinkedList`
-------------------------------- | ----------- | ------------
**Element insertion/removal**    | `O(n)`      | `O(1)`
**Random read access**           | `O(1)`      | `O(n)`

`LinkedList` allows for constant-time insertions or removals using iterators, but only sequential access of elements. In other words, you can walk the list forwards or backwards, but finding a position in the list takes time proportional to the size of the list.


`ArrayList`, on the other hand, allow fast random read access, so you can grab any element in constant time. But adding or removing from anywhere but the end requires shifting all the latter elements over, either to make an opening or fill the gap. Also, if you add more elements than the capacity of the underlying array, a new array (1.5 times the size) is allocated, and the old array is copied to the new one, so adding to an ArrayList is O(n) in the worst case but constant on average.

# References 
[Stackoverflow post](http://stackoverflow.com/questions/322715/when-to-use-linkedlist-over-arraylist) 