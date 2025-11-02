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