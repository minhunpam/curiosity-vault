## Algorithm - Circuit Equivalence based on SAT
1. Encode circuits ($C_1$ and $C_2$) into formulas ($\varphi_1$ and $\varphi_2$)
2. Compute the CNF of $\varphi_1 \oplus \varphi_2$
	- Use Tseitin Encoding
3. Give CNF($\varphi_1 \oplus \varphi_2$) to a SAT solver
4. $C_1$ and $C_2$ are equivalent iff $\varphi_1 \oplus \varphi_2$ is **UNSAT**

## Duality: Validity and Satisfiability
- $\phi$ is valid $\Leftrightarrow$ $\neg \phi$ is not satisfiable
- $\phi$ is satisfiable $\Leftrightarrow$ $\neg \phi$ is not valid

## Normal Forms
- Literal: propositional variable or its negation
- Disjunctive Normal Form (DNF)
	- Disjunction of conjunction of literals
- Conjunctive Normal Form (CNF)
	- Conjunction of disjunctino of literals
### DNF
- Enumerate satisfying models (output = 1), connect satisfying models with disjunctions
### CNF
- Excluse falsifying models by requiring that at least one literal must be different, connect with conjunctions

## Tseitin Encoding
- Use auxiliary variables -> Resulting formula in $\varphi' = Tseitin(\varphi)$ is not equivalent to $\varphi$

> **Definition of equisatisfiability**:
> $\phi$ and $\varphi$ are equisatisfiable $\Leftrightarrow$ either both are **satisfiable** OR both are **unsatisfiable**

## Tseitin Encoding
- Step 1: Assign new variables to each sub-formula 
- Step 2: Add explanation for each new variable
- Step 3: Apply Tseitin Rewrite Rules to obtain equisatisfiable CNF

## Derivation of Rewrite Rules
### 1. $r \leftrightarrow (p \land q) \Leftrightarrow (\neg r \land p) \land (\neg r \lor q) \land (\neg p \lor \neg q \lor r)$
$$
\begin{aligned}
	&\Leftrightarrow (r \to p \land q) \land (p \land q \to r) \newline
	&\Leftrightarrow (\neg r \lor (p \land q)) \land (\neg(p \land q) \lor r) \newline
	&\Leftrightarrow (\neg r \land p) \land (\neg r \lor q) \land (\neg p \lor \neg q \lor r)
\end{aligned}
$$
### 2. $r \leftrightarrow (p \lor q) \Leftrightarrow (\neg p \lor r) \land (\neg q \lor r) \land (\neg r \lor p \lor q)$

$$
\begin{aligned}
	&\Leftrightarrow (r \to p \lor q) \land (p \lor q \to r) \newline
	&\Leftrightarrow (\neg r \lor (p \lor q)) \land (\neg(p \lor q) \lor r) \newline
	&\Leftrightarrow (\neg r \lor p \lor q) \land ((\neg p \land \neg q) \lor r) \\
	&\Leftrightarrow (\neg r \lor p \lor q) \land (\neg p \lor r) \land (\neg q \lor r)
\end{aligned}
$$
### 3. $r \leftrightarrow \neg p \Leftrightarrow (\neg r \lor \neg p) \land (r \lor p)$ 
$$
\begin{aligned}
&\Leftrightarrow (r \to \neg p) \land (\neg p \to r) \\
&\Leftrightarrow (\neg r \lor \neg p) \land (p \lor r) \\
\end{aligned}
$$
## Examples
- Example 1:
![[LC - Tseitin Encoding - Example 1.png]]

- Example 2: 
![[LC - Tseitin Encoding - Example 2.png]]
