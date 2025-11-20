- Advantages:
	- Often small representation
	- Efficient manipulation
		- Boolean operations
	- Cannonical (unique) representation
		- If 2 formulas are semantically equivalent, then their BDD representations are equivalent
## Binary Decision Diagram (BDD)
### Definition
- Directed Acyclic Graph
- $(V \cup f \cup \{1\}, E)$
	- Internal Nodes $v \in V$
		- - Label: $l(v) \in \{x_1, \dots, x_n\}$
			- Variables of $f$
	- Function Node: $f$
		- represents Boolean Formula $f$
		- In-degree: 0
		- Out-degree: 1
	- Terminal Node: $1$
		- Constant Function: True
		- Out-degree: 0
	- Edges $E$
		- Else Edge
			- Marked with (empty) circle
			- Can have complement attribute (full cycle)

### Size of a BDD
- Worst case: exponential
	- If each internal node has 2 sub-BDDs, then BDD has $2^n - 1$ internal nodes
- Often: BDDs contain much redundancy
	- Obtain compact BDD by removing redundancies

### Initial Representation
![[LC - BDD - Intial Representation.png]]

- Example: Given a model: $\mathcal{M} := \{a \gets T, b \gets T, c \gets F, d \gets T\}$
	- Question: Does it hold that $\mathcal{M}  \models f$
> $\mathcal{M} \models f \Leftrightarrow$ its path in BDDs ends in terminal node $1$
   $\mathcal{M} \not\models f \Leftrightarrow$ its path in BDDs ends in terminal node $0$

### Representation f in DNF:
1. Top-down:
	- Enumerating all paths (models) that end in 1
![[LC - BDD - Represent f in DNF.png]]
### Final Representation:
#### Complemented Edge
![[LC - BDD - Complimented Edges.png]]
- A pointer that carries a "negation flag"
- Instead of storing a separate node to represent $\neg f$, the BDD uses the same node $f$ but marks the edge entering as it as "complemented"

#### Dangling Edges
![[LC - BDD - Dangling Edge.png]]
- An edge whose destination node is missing, deleted, uninitialized, or otherwise invalid

## Reduced Ordered BDD
1. No duplicate sub-BDDs
![[LC - ROBDD - no duplicate.png]]
2. No redundant nodes
![[LC - ROBDD - No redundant nodes.png]]

![[LC.png]]

3. Ordering on the variabls along any path
- A reduced and ordered BDD gives a canonical representation of a formula
## From Formula to BDD
- Enumerate satisfying models
	- Represented by path with even number of negations
	- Complement flips truth table
- Build DNF from satisfying models

1. Compute all Cofactors
	- Cofactor is boolean formula $f$ with respect to a variable $x$
		- Positive Cofactor $f_x$ : $f$ with $x$ to $\top$
		- Negative Cofactor: $f_{\neg x}$: $f$ with $x$ to $\bot$
	![[LC - Cofactors.png]]

2. Draw BDD from cofactors
	- 
3. Shift negations upwards
	- only shift the `true` branch, because negations should not be there