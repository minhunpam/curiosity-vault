## Conjunctive Normal Form
- **Literal**: propositional variable or its negation
	- E.g. : $p, \neg q$
- **Clause**: Disjunction of literals
	- E.g.: $(p \lor \neg q \lor r)$
--> **Conjunction of clauses:** $(p \lor \neg q \lor r) \land (\neg p \lor q \lor \neg r)$ 

- In Sat Solving, we need to convert formulas into CNF instead of DNF because:
	- Conversion in CNF is linear in time and size of the formula (number connectives)
	- Conversion in DNF is often exponential

### Notation:
- $\varphi$ is the formula in CNF - e.g.: $\varphi = (b \lor c) \land (\neg a \lor \neg b \lor c)$
- $A$ is an assignment of truth tables to variables
	- E.g.: $A = \{a \leftarrow T, b \leftarrow, c \leftarrow \bot\}$ 
	- Total or partial assignment
- $\varphi[A]$ == The formula $\varphi$ is evaluated under an assignment A, where all propositional variables in $\varphi$ are replaced by their truth values 
	- E.g.: $(\bot \lor \neg \bot) \land (\neg \top \lor \neg \bot \lor \bot) = \top$

## Basic Idea - Backtracking Binary Search
- Recursively search for a satisfying model/assignment
	- Search for A such that $\varphi[A] = \top$
	- If no such A exists
		- $\varphi$ is unsatisfiable
- Several optimizations to prune seach tree
	- Optimizations learn which paths do not have to be explored

### Terms
- **Decision** == algorithm assigns truth table to a variable
- **Decision Level** == number of currently made decisions
- **Decision heuristic** 
	- Heuristic to decide which variable should be assigned next
	- Huge impact on solving time

### Tabular Execution of the Basic Search
- Example: $\varphi := (\neg \lor b) \land (\neg \lor c) \land (\neg c \lor \neg a)$ 
- Decision heuristic
- In the table, we will the following rows:
	1. Step -> indicate which step you are in
	2. Decision level -> The current decision level (depth in the search tree)
	3. Assignment -> The current partical assignment  A
		⚠️***We apply the current partial assignment to the original clauses***
	4. CL1 - CLn -> The corresponding clauses after applying the current partial assignment
		- For a literal $l$ that appears in the current assignment and current considered clause -> The clause becomes `True` immediately -> ***Satisfied Clause***
		- For clauses containing $\neg l$ -> remove $\neg l$ 
			- For example: $A = \{a\}$, $\varphi = \neg a \lor b$
			 $\Rightarrow \varphi[A] = \bot \lor b = b$ 
			⚠️ In case of ***Empty/ conflicting clause***, we do a ***backtracking*** by undoing the last decision
	5. Decision -> Which literal we decided to set at this step
- If all clauses are satisfied, then report $A$ as satisfying assignment

## Boolean Constraint Propagation (BCP)
### Terms
- Unit clause:
	- has only one literal; it forces that literal to be True
	- Examples:
		- $a$ 
		- $\neg b$
- The table is showing each propagation **one at a time** per column
	- When SAT solver finds one unit clause first, it propages the literal 

### Purpose:
- to automatically infer which literals MUST be true from the current partial assignment, without making new decision
For example: $$\varphi = (\neg a \lor b) \land (\neg b \lor c) \land (\neg c \lor a)$$
- If decide $a = True$, then the first clause $(\neg a \lor b)$ becomes $b$ and BCP forces $b = True$
	- So that that point, the solver didn't decide b - it inferred b
	- That's why b goes under the BCP row in the table
	- b will be added to the next partial assignment

- Helps minimize the number of decisions a SAT solver needs to make by automatically deducing variable values implied by the current partial assignment
	- This reduces the search space and speeds up solving

## Pure Literal
- literal that appears only in one polarity throughout the entire formula
	- That means:
		- If $x$ appears only as $x$ (never as $\neg x$) -> $x$ is pure positive
		- If $x$ appears only as $\neg x$ (never as $x$) -> $x$ is pure negative

- If a literal is pure, then you can safely assign it in a way that satisfies all the clauses containing it, because there's no clause that would be contradicted by doing so

## Conflict-Driven Clause Learning (CDCL) + Conflict Graph

![[CDCL + Conflict Graph.png]] 

- Every time you encounter a conflict:
1. Draw conflict graph
	- The starting node is either a decision
2. Learn clause
	- The learning clause is derived from the resolution proof
	- **The learned clause could be used immediately for the BCP entry**
3. Add clause to set of clauses, backtrack and continue back as step 1 and 2 and ends until the resolution proof gives a False value aka UNSAT
	- backtrack as far as need, as possible where the learned clause is still unit clause

## Proof for Unsatisfiability
- Resolution Proof = Natural Deduction Proof using only the Resolution Rule
- Resolution Rule: Derive  natural deduction rule
$$
\frac
{(a \lor b_1 \lor \dots b_n) \quad (\neg a \lor c_1 \lor \dots c_m)} 
{(b_1 \lor \dots \lor b_n \lor c_1 \lor \dots \lor c_m)}
$$
- Proof Construction:
	- Turn **conflict graph** around
		- Select clause that implies conflict
		- Iteratively, resolve while back-traversing graph

## Assignments
### 1.

| Step                       | 1       | 2            | 3                | 4   | 5            | 6            | 7              | 8                      | 9                              | 10  | 11           | 12           | 13           | 14             | 15                  |
| -------------------------- | ------- | ------------ | ---------------- | --- | ------------ | ------------ | -------------- | ---------------------- | ------------------------------ | --- | ------------ | ------------ | ------------ | -------------- | ------------------- |
| Decision Level             | 0       | 1            | 1                | 0   | 0            | 0            | 1              | 1                      | 1                              | 0   | 0            | 0            | 0            | 1              | 1                   |
| Assignment                 | _       | $\neg a$     | $\neg a, \neg c$ | _   | a            | $a,c$        | $a, c, \neg b$ | $a, c, \neg b, \neg d$ | $a, c, \neg b, \neg d, \neg e$ | _   | $b$          | $a, b$       | $a, b, c$    | $a,b,c,\neg d$ | $a, b, c,\neg d, e$ |
| Clause 1: $a, \neg c$      | 1       | $\neg c$     | $\checkmark$     | 1   | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$           | $\checkmark$                   | 1   | $c, e$       | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 2: $b, c, e$        | 2       | 2            | $b, e$           | 2   | 2            | $\checkmark$ | $\checkmark$   | $\checkmark$           | $\checkmark$                   | 2   | $\checkmark$ | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 3: $b, \neg e$      | 3       | 3            | 3                | 3   | 3            | 3            | $\neg e$       | $\neg e$               | $\checkmark$                   | 3   | $\checkmark$ | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 4: $\neg a, c$      | 4       | $\checkmark$ | $\checkmark$     | 4   | $c$          | $\checkmark$ | $\checkmark$   | $\checkmark$           | $\checkmark$                   | 4   | 4            | $c$          | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 5: $d, e$           | 5       | 5            | 5                | 5   | 5            | 5            | 5              | $e$                    | $\times \{\}$                  | 5   | 5            | 5            | 5            | $e$            | $\checkmark$        |
| Clause 6: $b, \neg d$      | 6       | 6            | 6                | 6   | 6            | 6            | $\neg d$       | $\checkmark$           | $\checkmark$                   | 6   | $\checkmark$ | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 7: $\neg d, \neg e$ | 7       | 7            | 7                | 7   | 7            | 7            | 7              | $\checkmark$           | $\checkmark$                   | 7   | 7            | 7            | 7            | $\checkmark$   | $\checkmark$        |
| Clause 8: $a, c$           | 8       | c            | $\times \{\}$    | 8   | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$           | $\checkmark$                   | 8   | 8            | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 9: a                |         |              | learned a        | 9   | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$           | $\checkmark$                   | 9   | $a$          | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| Clause 10: b               |         |              |                  |     |              |              |                |                        | learned b                      | 10  | $\checkmark$ | $\checkmark$ | $\checkmark$ | $\checkmark$   | $\checkmark$        |
| BCP                        |         | $\neg c$     |                  | a   | $c$          |              | $\neg d$       | $\neg e$               |                                |     | $a$          | $c$          |              | $e$            |                     |
| PL                         |         |              |                  |     |              |              |                |                        |                                |     |              |              |              |                |                     |
| Decision                   | $\neg$a |              |                  |     |              | $\neg b$     |                |                        |                                |     |              |              | $\neg d$     |                | $SAT$               |
- The question can be solved with pure literal from step 1
- Conclusion: Satisfying model:; $\{a, b, c, \neg d, e\}$ 


### 2.

| Step                          | 1        | 2            | 3                | 4                        | 5            | 6            | 7                   | 8   | 9            | 10           | 11            |
| ----------------------------- | -------- | ------------ | ---------------- | ------------------------ | ------------ | ------------ | ------------------- | --- | ------------ | ------------ | ------------- |
| Decision level                | 0        | 1            | 2                | 2                        | 1            | 1            | 1                   | 0   | 0            | 0            | 0             |
| Assignment                    | _        | $\neg a$     | $\neg a, \neg b$ | $\neg a, \neg b, \neg f$ | $\neg a$     | $\neg a, b$  | $\neg a, b, \neg e$ | _   | $a$          | $a, b$       | $a, b$        |
| Clause 1: $\neg a, b$         | 1        | $\checkmark$ | $\checkmark$     | $\checkmark$             | $\checkmark$ | $\checkmark$ | $\checkmark$        | 1   | $b$          | $\checkmark$ | $\checkmark$  |
| Clause 2: $\neg d, c, \neg a$ | 2        | $\checkmark$ | $\checkmark$     | $\checkmark$             | $\checkmark$ | $\checkmark$ | $\checkmark$        | 2   | $\neg d, c$  | $\neg d, c$  | $\neg d, c$   |
| Clause 3: $\neg b, e$         | 3        | 3            | $\checkmark$     | $\checkmark$             | 3            | $e$          | $\times \{\}$       | 3   | 3            | $e$          | $\times \{\}$ |
| Clause 4: $\neg b, \neg e$    | 4        | 4            | $\checkmark$     | $\checkmark$             | 4            | $\neg e$     | $\checkmark$        | 4   | 4            | $\neg e$     | $\checkmark$  |
| Clause 5: $b, f$              | 5        | 5            | $f$              | $\times \{\}$            | 5            | $\checkmark$ | $\checkmark$        | 5   | 5            | $\checkmark$ | $\checkmark$  |
| Clause 6: $b, \neg f$         | 6        | 6            | $\neg f$         | $\checkmark$             | 6            | $\checkmark$ | $\checkmark$        | 6   | 6            | $\checkmark$ | $\checkmark$  |
| Clause 7: $a, b$              |          |              |                  | learned $a, b$           | $b$          | $\checkmark$ | $\checkmark$        | 7   | $\checkmark$ | $\checkmark$ | $\checkmark$  |
| Clause 8: $a$                 |          |              |                  |                          |              |              | learned $a$         | $a$ | $\checkmark$ | $\checkmark$ | $\checkmark$  |
| BCP                           |          |              | $\neg f$         |                          | $b$          | $\neg e$     |                     | $a$ | $b$          | $\neg e$     |               |
| PL                            |          |              |                  |                          |              |              |                     |     |              |              |               |
| Decision                      | $\neg a$ | $\neg b$     |                  |                          |              |              |                     |     |              |              | $UNSAT$       |
- On step 4, we have a conflict when $b \lor f$ becomes false, so the solve says the combination $(\neg a, \neg b, \neg f)$ can never happen again, it then creates a new learned clause to preven this situation: $a \lor b$ . That means: "at least one of a or b must be true."
- The solver goes back to fix the conflict by looking at which levels $a$ and $b$ were decided
	- a - level 1
	- b - level 2
-> The solver goes back to the earlier level (level 1), so that we apply the new clause + the assignment of $\neg a$, we can get something without taking $\neg b$

### 3. 

| Step                               | 1        | 2            | 3                | 4                   | 5                           | 6            | 7            | 8                   | 9                        |
| ---------------------------------- | -------- | ------------ | ---------------- | ------------------- | --------------------------- | ------------ | ------------ | ------------------- | ------------------------ |
| Decision level                     | 0        | 1            | 2                | 2                   | 2                           | 1            | 1            | 2                   | 2                        |
| Assignment                         | _        | $\neg a$     | $\neg a, \neg b$ | $\neg a, \neg b, c$ | $\neg a, \neg b, c, \neg d$ | $\neg a$     | $\neg a, b$  | $\neg a, b, \neg c$ | $\neg a, \neg b, \neg c$ |
| Clause 1: $a, b, c$                | 1        | $b, c$       | $c$              | $\checkmark$        | $\checkmark$                | $b, c$       | $\checkmark$ | $\checkmark$        | $\checkmark$             |
| Clause 2: $\neg b, \neg c, e$      | 2        | 2            | $\checkmark$     | $\checkmark$        | $\checkmark$                | 2            | $\neg c, e$  | $\checkmark$        | $\checkmark$             |
| Clause 3: $b, e$                   | 3        | 3            | $e$              | $e$                 | $e$                         | 3            | $\checkmark$ | $\checkmark$        | $\checkmark$             |
| Clause 4: $b, \neg d$              | 4        | 4            | $\neg d$         | $\neg d$            | $\checkmark$                | 4            | $\checkmark$ | $\checkmark$        | $\checkmark$             |
| Clause 5: $\neg c, d$              | 5        | 5            | 5                | $d$                 | $\times \{\}$               | 5            | 5            | $\checkmark$        | $\checkmark$             |
| Clause 6: $\neg c, e$              | 6        | 6            | 6                | $e$                 | $e$                         | 6            | 6            | $\checkmark$        | $\checkmark$             |
| Clause 7: $\neg a, \neg b, \neg c$ | 7        | $\checkmark$ | $\checkmark$     | $\checkmark$        | $\checkmark$                | $\checkmark$ | $\checkmark$ | $\checkmark$        | $\checkmark$             |
| Clause 8: $a, c, \neg e$           | 8        | $c, \neg e$  | $c, \neg e$      | $\checkmark$        | $\checkmark$                | $c, \neg e$  | $c, \neg e$  | $\neg e$            | $\checkmark$             |
| Clause 9: $a, b$                   |          |              |                  |                     | learned $a, b$              | $b$          | $\checkmark$ | $\checkmark$        | $\checkmark$             |
| BCP                                |          |              | $c$              | $\neg d$            |                             | $b$          |              | $\neg e$            |                          |
| PL                                 |          |              |                  |                     |                             |              |              |                     |                          |
| Decision                           | $\neg a$ | $\neg b$     |                  |                     |                             |              | $\neg c$     |                     | $SAT$                    |

### 4. 

| Step                          | 1        | 2            | 3                | 4                   | 5                           |
| ----------------------------- | -------- | ------------ | ---------------- | ------------------- | --------------------------- |
| Decision level                | 0        | 1            | 2                | 2                   | 2                           |
| Assignment                    | _        | $\neg x$     | $\neg x, \neg y$ | $\neg x, \neg y, w$ | $\neg x, \neg y, w, \neg z$ |
| Clause 1: $x, y, w$           | 1        | $y, w$       | $w$              | $\checkmark$        | $\checkmark$                |
| Clause 2: $\neg x, z, \neg w$ | 2        | $\checkmark$ | $\checkmark$     | $\checkmark$        | $\checkmark$                |
| Clause 3: $x, \neg y, w$      | 3        | $\neg y, w$  | $w$              | $\checkmark$        | $\checkmark$                |
| Clause 4: $\neg z, \neg w$    | 4        | 4            | 4                | $\neg z$            | $\checkmark$                |
| Clause 5: $\neg y, z$         | 5        | 5            | $\checkmark$     | $\checkmark$        | $\checkmark$                |
| Clause 6: $\neg x, y, w$      | 6        | $\checkmark$ | $\checkmark$     | $\checkmark$        | $\checkmark$                |
| BCP                           |          |              | $w$              | $\neg z$            |                             |
| PL                            |          |              |                  |                     |                             |
| Decision                      | $\neg x$ | $\neg y$     |                  |                     | $SAT$                       |



### 5.
| Step                       | 1        | 2            | 3                | 4                   | 5                           | 6   | 7            | 8            | 9                   | 10                     | 11                             |
| -------------------------- | -------- | ------------ | ---------------- | ------------------- | --------------------------- | --- | ------------ | ------------ | ------------------- | ---------------------- | ------------------------------ |
| Decision level             | 0        | 1            | 1                | 1                   | 1                           | 0   | 0            | 0            | 0                   | 0                      | 0                              |
| Assignment                 | _        | $\neg a$     | $\neg a, \neg c$ | $\neg a, \neg c, b$ | $\neg a, \neg c, b, \neg d$ | _   | $a$          | $a, \neg e$  | $a, \neg e, \neg c$ | $a, \neg e, \neg c, b$ | $a, \neg e, \neg c, b, \neg d$ |
| Clause 1: $\neg b, c, d$   | 1        | 1            | $\neg b, d$      | $d$                 | $\times \{\}$               | 1   | 1            | 1            | $\neg b, d$         | $d$                    | $\times \{\}$                  |
| Clause 2: $\neg b, \neg d$ | 2        | 2            | 2                | $\neg d$            | $\checkmark$                | 2   | 2            | 2            | 2                   | $\neg d$               | $\checkmark$                   |
| Clause 3: $a, \neg c$      | 3        | $\neg c$     | $\checkmark$     | $\checkmark$        | $\checkmark$                | 3   | $\checkmark$ | $\checkmark$ | $\checkmark$        | $\checkmark$           | $\checkmark$                   |
| Clause 4: $\neg c, e$      | 4        | 4            | $\checkmark$     | $\checkmark$        | $\checkmark$                | 4   | 4            | $\neg c$     | $\checkmark$        | $\checkmark$           | $\checkmark$                   |
| Clause 5: $b, c$           | 5        | 5            | $b$              | $\checkmark$        | $\checkmark$                | 5   | 5            | 5            | $b$                 | $\checkmark$           | $\checkmark$                   |
| Clause 6: $\neg a, \neg e$ | 6        | $\checkmark$ | $\checkmark$     | $\checkmark$        | $\checkmark$                | 6   | $\neg e$     | $\checkmark$ | $\checkmark$        | $\checkmark$           | $\checkmark$                   |
| Clause 7: $a$              |          |              |                  |                     | Learned $a$                 | $a$ | $\checkmark$ | $\checkmark$ | $\checkmark$        | $\checkmark$           | $\checkmark$                   |
| BCP                        |          | $\neg c$     | $b$              | $\neg d$            |                             | $a$ | $\neg e$     | $\neg c$     | $b$                 | $\neg d$               |                                |
| PL                         |          |              |                  |                     |                             |     |              |              |                     |                        |                                |
| Decision                   | $\neg a$ |              |                  |                     |                             |     |              |              |                     |                        | $UNSAT$                        |






