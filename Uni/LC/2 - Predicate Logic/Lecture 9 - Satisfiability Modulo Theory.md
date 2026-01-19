## Notion of a Theory
- **Theory** defines axioms for often used application/ problem domains
- Definition of a First-order Theory $\tau$   :
	- Signature $\Sigma$
		- is a set of constants, predicate and function symbols
		- A formula $\varphi$ in $\tau$ only uses non-logical symbols contained $\Sigma$ 
			- Additionally, $\varphi$ can contain logical symbols, variables like $x, y$ ..., operators: $\lor$, $\neg$ 
	- Set of axioms $\mathcal{A}$ 
		- Gives meaning to the predicate and function symbols
		- **Axiom = Formula without free variables with symbols from $\Sigma$ only**

### Theory of Linear Integer Arithmetic $\tau_{\text{LIA}}$
Example: $\varphi:= x \geq 0 \land (x + y \leq 2 \lor x + y \geq 6)$
- **Definition of  $\tau_{\text{LIA}}$**: $\Sigma_{LIA} := \mathbb{Z} \cup \{+, -\} \cup \{=, \not = , <, \leq, \geq, \geq\}$

### Theory of Equality & Uninterpreted Functions $\tau_{EUF}$
- **Arity** - the number of arguments of a function symbol takes
	- Examples:
		- `f(x)` -> arity = 1 (unary function)
		- `g(x, y)` -> arity = 2 (binary function)
		- `c()` -> arity = 0 (this is a **constant symbol**)
	- Arity matters because **it is part of the identity of the function symbol**
- **Function congruence property**
	- for a function `f` of arity `n`:
	- $(a_1 = b_1 \land a_2 = b_2 \land \dots a_n = b_n) \to f(a_1, a_2, \dots , a_n) = f(b_1, b_2, \dots , b_n)$ == **If you give the function exactily the same inputs, it must produce the same output**

## Ackermann's reduction
- The rule requires:
> If 2 applications of `f` have equal arguments (component-wise) then their results must be equal

For a binary function `f(u, v)` this means:
$$
(u_1 = u_2 \land v_1 = v_2) \to f(u_1, v_1) = f(u_2, v_2)
$$

## Lazy Encoding
### The problem with Eager encoding:
- Suppose we want to solve formulas with: Boolean structure, Equalities, Uninterpreted functions. **Eager encoding would encode everything into SAT before solving**
- For EUF, this means:
	1. Every pair of terms gets a Boolean variable $t_i = t_j$
	2. Add transitivity axioms
	3. Add congruence axioms for functions
- This costs:
	- $O(n^2)$ equalities
	- Huge number of clauses
--> **Lazy encoding avoids this explosion**


## Theory Solver for $\tau_{UE}$ - Congruence-Closure Algorithm
1. For every equality, create a congruence class + create a singleton class for every term that only appears in disequalities
		- Example: $t_1 = t_2$ : create class for $t_1, t_2$
2. Merge class:
	- Shared term between classes! (repeat)
	- $t_i, t_j$ from same class: Merge classes of $f(t_i), f(t_j)$ (repeat)
	- No merging possible anymore, go to step 3
3. Check disequalities $t_k \not = t_l$ 
	- $t_k, t_l$ in the same class: **UNSAT**
	- Otherwise: **SAT**

## Assignments
### Exercise 4

| Step                           | 1     | 2            | 3            | 4               | 5                    | 6                              |
| ------------------------------ | ----- | ------------ | ------------ | --------------- | -------------------- | ------------------------------ |
| Decision level                 | 0     | 1            | 1            | 1               | 1                    | 1                              |
| Assignment                     | _     | $e_0$        | $e_0, e_1$   | $e_0, e_1, e_2$ | $e_0, e_1, e_2, e_4$ | $e_0, e_1, e_2, e_4, \neg e_3$ |
| Cl.1: $e_0 \lor e_1 \lor e_2$  | 1     | $\checkmark$ | $\checkmark$ | $\checkmark$    | $\checkmark$         | $\checkmark$                   |
| Cl.2: $\neg e_0 \lor e_1$      | 2     | $e_1$        | $\checkmark$ | $\checkmark$    | $\checkmark$         | $\checkmark$                   |
| Cl.3: $\neg e_1 \lor e_2$      | 3     | 3            | $e_2$        | $\checkmark$    | $\checkmark$         | $\checkmark$                   |
| Cl.4: $e_2 \lor e_3$           | 4     | 4            | 4            | $\checkmark$    | $\checkmark$         | $\checkmark$                   |
| Cl.5: $\neg e_2 \lor e_4$      | 5     | 5            | 5            | $e_4$           | $\checkmark$         | $\checkmark$                   |
| Cl.6: $\neg e_3 \lor \neg e_4$ | 6     | 6            | 6            | 6               | $\neg e_3$           | $\checkmark$                   |
| BCP                            |       | $e_1$        | $e_2$        | $e_4$           | $\neg e_3$           |                                |
| PL                             |       |              |              |                 |                      |                                |
| Decision                       | $e_0$ |              |              |                 |                      | $SAT$                          |
### Exercise 5

| Step                                                           | 1     | 2            | 3            | 4                    | 5                              | 6                                   | 7          | 8            | 9               | 10                   | 11                        |
| -------------------------------------------------------------- | ----- | ------------ | ------------ | -------------------- | ------------------------------ | ----------------------------------- | ---------- | ------------ | --------------- | -------------------- | ------------------------- |
| Decision level                                                 | 0     | 0            | 1            | 1                    | 1                              | 1                                   | 0          | 1            | 1               | 1                    | 1                         |
| Assignment                                                     | _     | $e_5$        | $e_5, e_0$   | $e_5, e_0, \neg e_1$ | $e_5, e_0, \neg e_1, \neg e_3$ | $e_5, e_0, \neg e_1, \neg e_3, e_2$ | _          | $\neg e_0$   | $\neg e_0, e_1$ | $\neg e_0, e_1, e_3$ | $\neg e_0, e_1, e_3, e_5$ |
| Cl.1: $e_0 \lor e_1$                                           | 1     | 1            | $\checkmark$ | $\checkmark$         | $\checkmark$                   | $\checkmark$                        | 1          | $e_1$        | $\checkmark$    | $\checkmark$         | $\checkmark$              |
| Cl.2: $e_2 \lor e_3 \lor e_4$                                  | 2     | 2            | 2            | 2                    | $e_2 \lor e_4$                 | $\checkmark$                        | 2          | 2            | 2               | $\checkmark$         | $\checkmark$              |
| Cl.3: $e_5 \lor \neg e_2 \lor \neg e_4$                        | 3     | $\checkmark$ | $\checkmark$ | $\checkmark$         | $\checkmark$                   | $\checkmark$                        | 3          | 3            | 3               | 3                    | $\checkmark$              |
| Cl.4: $\neg e_0 \lor \neg e_1$                                 | 4     | 4            | $\neg e_1$   | $\checkmark$         | $\checkmark$                   | $\checkmark$                        | 4          | $\checkmark$ | $\checkmark$    | $\checkmark$         | $\checkmark$              |
| Cl.5: $e_1 \lor \neg e_3$                                      | 5     | 5            | 5            | $\neg e_3$           | $\checkmark$                   | $\checkmark$                        | 5          | 5            | $\checkmark$    | $\checkmark$         | $\checkmark$              |
| BC.6: $\neg e_5 \lor \neg e_0 \lor e_1 \lor e_3 \lor \neg e_2$ |       |              |              |                      |                                |                                     | 6          | $\checkmark$ | $\checkmark$    | $\checkmark$         | $\checkmark$              |
| BCP                                                            |       |              | $\neg e_1$   | $\neg e_3$           |                                |                                     |            | $e_1$        |                 |                      |                           |
| PL                                                             | $e_5$ |              |              |                      | $e_2$                          |                                     |            |              | $e_3$           | $e_5$                |                           |
| Decision                                                       |       | $e_0$        |              |                      |                                |                                     | $\neg e_0$ |              |                 |                      |                           |
