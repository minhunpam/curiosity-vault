## Catalan Numbers
- are sequence of positive numbers that appear in many couting problems in combinatorics
- The Catalan numbers $C_0, C_1, \dots, C_n, \dots$ are given by the formula
$$
C_n = \frac{1}{n+1}{2n \choose n}
$$
### Recurrence Relation
- The Catalan numbers satisfy the recurrence relation
$$
C_{n+1} = C_0C_n + C_1C_{n-1} + \dots + C_nC_0 = \sum_{k = 0}^{n}C_kC_{n-k}
$$
#### Rooted binary tree (Full binary tree)
- 1 root node, where each node has either 0 or 2 branches descending from it
- A node is internal if it has 2 nodes coming from it
- Let $R_n$ be the number of rooted binary trees with $n$ internal nodes
	- $R_0 = R_1 =1$
	- For $n \geq 0$ , suppose there is a rooted binary tree with $n + 1$ internal nodes
		- The root node is internal
		- There are $$