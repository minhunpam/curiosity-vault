- The shortest path in a graph is not unique
- If the graph is an abitrary tree/ binary tree, the shortest path and the shortest path length is unique, that's why we don't compute "all shortest paths"
## Basic Terminology
- **Directed graph** $G = (V, E)$
	- 

## Breadth First Search
- State of a vertex in the graph
	- Its distance to the root
	- Its nearest parent (where it stemmed from)
		- We could use this property to find backtrack and find the path from it back to the root
	- `IsVisited` flag
	- Runtime: $\Theta(n+m)$ if the graph is connected
## Depth First Search

## How to store a graph?
- **Adjacency-matrix**:
	- Matrix $A[1\dots n][1\dots n]$  with:
$$
A[i][j] = 
\begin{cases}
1 \text{ if (i, j) } \in E \\
0 \text{ else}
\end{cases}
$$
- Memory: $\Theta(n^2)$
- Convenient for dense graphs
- Test for existence of a edge in $\Theta(1)$ time
- Runtime:
	- Initialization: $\Theta(n^2)$
	- Filling the tree: $\Theta(n)$

- **Adjacency List**:
	- An array, where each entry points to a linear list with all vertices
	- 