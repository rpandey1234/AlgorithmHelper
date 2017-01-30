# Trees

A tree is a collection of individual entries called nodes such that:

- There exists a unique node called the root node that forms the top of the hierarchy. 
- Every other node is connected to the root by a unique line of descent. 

A binary tree is a tree with the following additional properties:

- Each node in the tree has at most two children. 
- Every node except the root is designated as a left or right child of its parent. 

A binary search tree (BST) also obeys:

- Every node contains a key which defines the order of the nodes. 
- Key values are unique. 
- At every node in the tree, the key value must be greater than all the keys in the left subtree and less than all the keys in the right subtree. 

In pre-order traversal, the current node is processed *before* either of the subtrees. In post-order traversal, the current node is processed *after* the subtrees. 

The running time of a tree traversal algorithm is proportional to the height of the tree, which is ideally `O(log n)` if the tree is *balanced*. A binary tree is deemed balanced if, at each node, the height of the left and right subtrees differ by at most one. Two types of self-balancing trees are AVL trees and red-black trees.

### References
- [BST on WIkipedia ](https://en.wikipedia.org/wiki/Binary_search_tree)
