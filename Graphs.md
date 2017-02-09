# Graphs

Given graph G, denote the number of edges with `m`, and the number of nodes with `n`. Suppose we have an undirected graph, with only one edge between each node. The densest graph has `m = n C 2 = n(n-1)/2` edges. In a directed graph, we could have twice the number of edges, `n(n-1)`. In either case, m = O(n<sup>2</sup>).

If `G` is connected, then `m ≥ n-1`.

There are 2 ways to represent a graph:

- Adjacency Matrix: Represent `G` with an `n by n` matrix A, where A<sub>ij</sub> is 1 iff `G` has an `ij` edge. This has O(n<sup>2</sup>) space requirement.
- Adjacency List: There is an array of vertices. Every vertex stores a list of adjacent vertices. This data structure allows the storage of additional data on the vertices. The total space requirement is `O(m+n)`.

Adding/removing a vertex to a graph is constant time in adjacency lists, while it takes O(n<sup>2</sup>) time in an adjacency matrix. Adjacency lists are slower in querying if two vertices are adjacent (require `O(m)` time compared to `O(1)`). Adjacency lists are generally preferred because they efficiently represent sparse graphs.

### Graph Connectivity
Graph Primitives: breadth first search (BFS) and depth first search (DFS)

- BFS is where you explore a graph layer by layer, in a cautious, tentative way. Keep track of the nodes using a queue, explore the neighbors of a particular node, and only after exploring all those neighbors you explore neighbors that are distance 2 away, and so on. 
- DFS is a more aggressive search, similar to how you would explore a maze. Keep track of the next node using a stack, or a recursive loop. 

Both DFS and BFS run in `O(m+n)` time, since each node is only visited once. 

### Dijkstra's Algorithm
Dijkstra's Algorithm solves the single source shortest path (SSSP) problem. We are given a directed graph `G` where each edge has length c<sub>e</sub>, and a source vertex <em>s</em>. For each possible destination `v` in `V`, compute the length `D(v)` of the shortest s-v path in G. Note that we assume that all edge lengths are nonnegative, and there exists a path from `s` to `v` for all `v` in `V`. (We employ another technique, Bellman-Ford, if the graph has negative edge lengths.)

The idea behind Dijkstra is to start somewhere and grow as a mold, eventually covering the entire graph. We prove this by induction. 


### Strongly Connected Components
In a directed graph G, a strongly connected component (SCC) is a set of vertices S such that there is a path from each vertex in S to every other vertex in S.

If each SCC is contracted to a single vertex, the resulting graph is a directed acyclic graph (DAG), the condensation of G. (If the resulting graph was not a DAG, then the resulting cycle could be used to collapse into a single SCC.)

We can use Kosaraju's Algorithm for finding SCCs. Kosaraju's Algorithm is a linear time algorithm to find the strongly connected components of a directed graph. It works by marking each vertext in the graph as unvisited, then performing a DFS traversal of the graph. Next, we reverse the direction of all edges in the graph to get the transpose graph. Now we process the vertices in the reverse order from the earlier traversal, and perform a DFS on each node. The set of reachable nodes from these set of DFS traversals will be your set of SCCs. 

# References
- [Wikipedia page for Kosarju's Algorithm](https://en.wikipedia.org/wiki/Kosaraju's_algorithm)
- [Geeks for geek Kosaraju's Algorithm](http://www.geeksforgeeks.org/strongly-connected-components/)