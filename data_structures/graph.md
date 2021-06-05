# Graph

## Definition

A collection of nodes N and edges E. Where N is a non-empty set. And E is a set of pair of elements from N.

Cardinality of a set S is denoted by |S| and is equal to the total number of elements in a set.

The time complexity of graph traversal is |N| + |E| using BFS or DFS.

### Bipartite Graph

If nodes of the graph can be split in two exhaustive non-overlapping sets so that a edge exist from each element of set 1 to atleast 1 element of set 2 and non of the edges exist within set 1 or set 2, i.e. if no element in set 1 have and edge connecting it to an element in same set.

If a graph have odd size cycle then a graph can't be bipartite. On the contrary, if a graph is asyclic or a graph have only even size cycles it is a bipartite graph.

**Are the graphs with no vertex and 1 vertex bipartite ?**

**Yes, any graph with no edges is trivially bipartite.**

Again, a bipartite graph (or bigraph) is a graph whose vertices can be divided into two disjoint sets U and V such that every edge connects a vertex in U to one in V; that is, U and V are independent sets. Equivalently, a bipartite graph is a graph that does not contain any odd-length cycles.

[HOME](./../README.md)
