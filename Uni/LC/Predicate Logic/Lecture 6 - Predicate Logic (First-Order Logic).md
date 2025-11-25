## Limitation of Propositional Logic
- Propositional Logic cannot express quantified variables
	- "**for all** cats..."
	- "**there exists** a person..."
- Additionally
	- We want to use variables over **Integer, Reals**
	- We want to use **functions and predicates** over these domains

## Formulas in Predicate Logic
$$
\Large
\forall x \exists y. P(x, f(x,y))
$$
- Variables: $x, y$ 
	- over arbitrary domains
		- Examples: Integers, Reals, people, countries
- Predicates: $P$
	- **A predicate maps variables to a Boolean value**
- Functions: $f$
	- A function maps variables to a value in an **arbitrary domain**
- Universal Quantification: $\forall x$
	- $\forall x P(x)$ is true for all possible values of x

## Modelling Sentences in Predicate Logic
- Every even integer greater than 2 is equal to the sum of 2 prime numbers
- S
- Domain: $\mathcal{A} = \mathbb{Z}$ 
- Predicates:
	- $Even(x)$ ... `true` if $x$ is even
	- $x > 2$ ... returns `true` if $x > 2$
	- $isPrime(x)$ ... returns `true` if x is a prime number
	- $x = y$ ... `true` if $x$ is equal to `y`
- Functions:
	- `x + y` ... returns the the sum of x and y

$$
\forall x \in \mathbb{Z} \exists a \in \mathbb{P} \exists b \in \mathbb{P}(x > 2 \land x \equiv 2 \mod 0 \to x = a + b)
$$
## Syntax of Predicate Logic
- Terms
	- Refer to objects of the domain
		- Constants
		- Variables
		- Functions
	- Any variable $v \in \mathbb{F}$ is a term
	- If $c \in F$ is a nullary function (no argument),  then $c$ is a term
	- Given terms $t_1, t_2, \dots, t_n$ and an n-ary function symbol $f \in \mathbb{F}$ , then $f(t_1, t_2, \dots, t_n)$ is a term
- Formulas
	- Have a truth table
		- Predicates
		- Boolean vars and constants
	- Given $t_1, t_2, \dots, t_n$ and an  n-ary predicate symbol $P \in \mathbb{P}$, then $P(t_1, t_2, \dots, t_n)$ is a formula
	- If $\varphi$ and $\psi$ are formulas, then $\neg \varphi, (\varphi \land \psi), (\varphi \lor \psi), (\varphi \to \psi)$ are formulas
	- If $\varphi$ is a formula and $x \in \mathbb{V}$ is a variable, then ($\forall x \phi$  and $\exists x \phi$ ) are formulas
### Notation
- Set of variable: $\mathbb{V}$
- Set of functions: $\mathbb{F}$
- Set of predicates: $\mathbb{P}$
### Binding Priorities
1. $\forall, \exists, \neg$
2. $\land$
3. $\lor$
4. $\to$

### Free and bound variables
- A free variable - an instance of $x$ in $\varphi$ if it's not in the scope of a $\forall x$ or $\exists x$
- A bound variable, otherwise

## Model $M$ for formulas in Predicate Logic
- Models for predicate logic formulas need to define
	- Domain of variables
	- Values for free variables
	- Values for nullary functions
	- Truth tables for nullary predicates
	- Concrete instances for any function and predicate

## Semantics of Predicate Logic
- We want to know if $M$ satisfies $\varphi$ ($M \models \varphi$)
- For $\varphi$ of the form $\forall x \psi$ 
	- $M \models \forall x \psi$ iff $M_{[x \leftarrow a]} \psi$, for all $a \in A$
- For $\varphi$ of the form $\exists x \psi$
	- $M \models \forall x \psi$ iff  $M_{[x \leftarrow a]} \psi$, for at least one $a \in A$
> $[x \leftarrow a]$ means that $x$ is mapped to $a$
- For $\varphi$ of the form $\neg \psi, $\psi_1 \land \psi_2, \psi_1 \lor \psi_2, \psi_1 \to \psi_2$
	- Like in propositional logic


