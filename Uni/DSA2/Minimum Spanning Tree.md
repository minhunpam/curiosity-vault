## Spanning Tree
- A spanning tree is a tree that has $n$ vertices and $n -1$ edges -> form no cycle 


- A Euclidean minimum spanning tree of a set $S$ of points is a tree that connects all points and minimized the total edge length among all trees in $S$
- A minimum spanning tree of a weighted graph $G = (V, E, \omega)$ is a tree $T = (V,E')$ with $E' \subseteq E$ and with minimal total edge length among all spanning trees in G:
$$\Large
w(T) = \Sigma_{e \in E'} w(e)
$$
is minimized over all trees in $G$ with vertex set $V$

## Iterative Algorithm Idea to build MST for a graph
- Inserting edges

```
initialize the empty edges for the MST - E'
loop until the |E'| < n -1
    select edge from E that has not been in E' which is 'good' for edges in E'
    add the selected edge to E'
```

## Cuts in graphs
- A cut of a graph $G=(V, E)$ is a partition of $V$ into $V_1, V_2$
	- An edge $e$ is called **external** for the cut ($V_1, V_2$) if it has one endpoinit in $V_1$ and one in $V_2$
	- otherwise $e$ is called **internal**
  ![[DSA - cuts in graphs.png]]

### Characterization of good edges
- **Theorem:** Let $E'$ be subset of edges of an MST of $G = (V, E, w)$. Let $(V_1, V_2)$ be a cut of $G$ for which all edges of $E'$ are internal. Then the external edges of the cut with ***mininum weight*** is a good edge for $E'$
#### Proof:
- Assume there is an MST $T$ that doesn't contain $e$
- A spanning tree has exactly $n -1$ edges. If one more edge is added to it, it will become a cycle $C$
- Since the cut separates $V_1$ and $V_2$, any closed loop must cross the cut an even number of times
	- Thus, the cycle contains 2 crossing edge $e$ and $f$
- Because $e$ is the lightest crossing edge ($w(f) \geq w(e)$), we remove $f$ and keep $e$, we get $T' = T \setminus \{f\} \cup \{e\}$ , the result again becomes a spanning tree
- Compare weights:
$$
w(T') = w(T) - w(f) + w(e) \leq w(T)
$$
> **Remarks**: If $w(f) > w(e)$ then $T$ is not an MST

## Prim's algorithm
- Start with an arbitrary vertex $s$ of $G$ and iteratively 'grow' an MST $T$ from $s$
- Iterative step:
	- Choose the 'cheapest' edge with exactly one node in T
	- Cut:
		- $V_1$ = vertices of $T$
		- $V_2$ = vertices not yet in $T$
	- For each vertex $v \not \in T$
		- Priority $p(v)$: weight of the shortest edge from $v$ to a vertex in $T (\text{initialy: } \infty)$
		- Nearest $n(v)$: vertex in $T$ realizing $p(v)$: $w(v, n(v))$ is minimum among neighbors of $v$ in $T$ (initially no vertex)
			- $n(v$)$ is the vertex inside the tree that is closest to $v$
	- A queue $Q$ contains all vertices not yet in $T$, organized by priorities (e.g., in a min-heap; intially all vertices)

```
PRIM-MST(Graph, arbitrary root)
for all v in V do p(v) = infinity od -------> at the beginning, we haven't connected anything yet, so we pretend the cost is "infinitely large" for                                                all

// Special treatment of the start vertex
p(s) = 0 -----------> we start from s
n(s) = nil ---------> s has no parent in the tree (it's the root)

// Build the priority queue
Q = V

while Q # 0 do ------------> Each iteration, we add 1 vertex to the tree
    u = MIN(Q) ------------> Take the cheapeast vertex (with the smallest p(v))
               ------------> For the start iteration, it is the root itself
               
	remove u from Q ---------------------> reorganize Q
	write u, n(u)
	
	// Traverse the neighbors of u
	for all v in AdjacencyList(u) do
		if v in Q and p(v) > w(u, v) then
			p(v) = w(u, v) --------------------> reorganize Q
			n(v) = u
		fi
	od
od
```

### Runtime Analysis
- $n$ ... number of vertices of $G$
- $m$ ... number of edges of $G$
- $d(v)$ ... degree of vertex $v$ in $G$

- Initialization, construction of the priority queue $Q$: $\Theta(n)$ 
- $n$ times removing the minimum from $Q$: $O(nlogn)$
- Report MST edges: $\Theta(n)$
- Update priorities for all neighbors of $v$: $O(d(v) \cdot log n$
--> Altogether: $O(n + n logn + \Sigma_{v \in V} d(v)logn) = O(m log n)$

Memory requirements: $\Theta(m + n) = \Theta(m)$

> Remarks: 
> - MST always begins at the start vertex $s$ and grows from there as a connected tree
> - Shrinking $p(v)$ causes $v$ to move up in the heap
> - Test for $v \in Q$ in $O(1)$ when bit vector is used to store which vertices are already in $T$
> - Runtime can be changed to $O(n^2)$. This is useful for dense graphs

## Kruskal's algorithm
- Start with empty edge set $E'$
- Sort edge set $E$ of $G = (V, E, w)$ in increasing order of their weights (edges will be considered in this order):
	- $e_1, e_2, \dots, e_m$ with $w(e_1) \leq w(e_2) \leq \dots \leq w(e_m)$
- Iterative step:
	- $E'$ forms a forest $F$ (= set of disjoint subtrees, acyclic) in $G$ and in the $MST$ to be constructed
	- Edge $e$ that is added to $E'$ is the shortest edge in $E\setminus E'$ that does not form a cycle with edges from $E'$
- Use **UNION-FIND** data structure on $V$ for the components (subtrees) $M_1, M_2,\dots , M_t$ of $F$:
	- Label the vertex set of $G$ as $v_1, v_2, \dots, v_n$ (arbitrary)
	- Initially there are $n$ disjoint sets $M_1, M_2, \dots , M_n$ (each with 1 vertex)
	- `FIND(v)`: returns index $i$ if vertex $v$ is in $M_i$
	- `UNION(i, j)`: join sets $M_i$ and $M_j$ : $M_i = M_i \cup M_j$ (index of resulting set: minimum of $i$ and $j$)
	- End of the algorithm: one component $M_1$ with all vertices of $G$
- 