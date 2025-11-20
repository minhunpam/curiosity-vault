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
\forall x \in \mathbb{Z} \exists a \in \mathbb{P} \exists b \in \mathbb{P}(x > 2 \land x \equiv 2 (mod 0) \to x = a + b)
$$
## Syntax of Predicate Logic
- Terms
	- Refer to objects of the domain
		- Constants
		- Variables
		- Functions
- Formulas
	- Have a truth table
		- Predicates
		- Boolean vars and constants
### Notation
- Set of variable: $\mathbb{V}$
- Set of functions: $\mathbb{F}$
- Set of predicates: $\mathbb{P}$
- 

### Binding Priorities
1. $\forall, \exists, \neg$
2. $\land$
3. $\lor$
4. $\to$

### Free and bound variables
- A free variable - an instance of $x$ in $\varphi$ if it's not in the scope of a $\forall x$ or $\exists x$
- A bound variable, otherwise

