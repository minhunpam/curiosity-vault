Dynamic Programming Stack:
- Define Sub-problems
- Recursive Relation
- Base Case(s)
- Topological Order
- Original Problem

Pseudocode would like:
```
base case(s)

iterate through the topological order
	solve s(i) by applying recursive relation

return solution to original problem computed by solutions of subproblems
```

## Bowling Problem
- Input: Given a sequence of bowling pins, numbered with integers $\in [-K, K]$
- You can:
	- hit single pins with number $x_i$ -> You get $x_i$ points
	- hit 2 adjacent pins $x$ and $y$ -> You get $x.y$  points
	- Throw as many balls as you want (**Don't have to hit all pins**)
- Goal: points of different balls add up

```
Example:
(3) (7) (8) (-1) (-3) (-2)
 1   2   3    4    5    6
```
### Idea: 
- Before actually throwing the ball, we have 3 options with respect to a turn:
	1. Choose not to throw that pin
	2. Only throw that pin
	3. Throw that pin and the one before that
	-  And our goal is maximize the value -> $max\{\text{Option 1}, \text{Option 2}, \text{Option 3}\}$ 

### Base Case:
- If there are no bowling pins, there would be no points -> $dp[0] = 0$
- For the first bowling pin, because we cannot move backward 2 indices, so when considering that bowling pin, we only take 2 option, the first and the second -> $max\{\text{Option 1}. \text{Option 2}\}$

## Recursion Relation
$$
dp[i] =
\begin{cases}
	0& \text{if i} = 0 \\
	max\{dp[0], dp[0] + x_1\}& \text{if i} =1 \\
	max\{dp[i-1], dp[i - 1] + x_i, dp[i-2] + x_{i-1}.x_{i}\}& \text{if i} \geq 2
\end{cases}
$$



## Knapsack problem 
- Input: 
	- n items
		- each item has some weight + profit
	- a bag with capacity W -> the bag can hold at most W weight in it
- Goal: put the items into the bag such that the sum of profits is the maximum possible
> Note: 

## Max Subarray sum
- input: `int A[1,...,n]`
- continuous subarray
- `A[i:j]` with the max sum

### Method 1: Check all subsets of A
- continuous?
- what is it its sum? New max
	-> O(n)
- #(subsets): 2^n

### Method 2: Focus on continuous subarrays
- Runtime: O(n^3)

### Method 3: 


## Topological Order
### Directed Acyclic Graph #GraphTheory
- a directed graph with no directed cycles
- consists of vertices and edges, with each edge directed from 1 vertex to another, such that following those directions will never form a closed loop

### Topological Sort/ Topological Ordering
- topological sort is a graph traversal in which each node v is visited only after all its dependencies are visited
![[directed acyclic graph.png]]