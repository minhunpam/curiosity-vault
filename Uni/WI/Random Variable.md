- A random variable is a real-valued variable whose value is determined by an underlying random experiment
- In essence, a random variable is a real-valued function that assigns a numerical value to each possible outcome of the random experiment

## Definition:
- A random variable $X$ is a function that maps from the sample space to the real numbers
$$
X: \Omega \to \mathbb{R}
$$
- Range of a random variable $X$, shown by $R_{X}$ , is the set of possible values for $X$

## Examples:
- We roll 2 dices and are interested in the sum of faces
	- $\Omega = \{\omega = (\omega_1, \omega_2) \in \{1,\dots,6\}^2\}$
	- $X = \{\omega : \omega = \omega_1 + \omega_2\}$
	- $\{X = 4\} = \{(1, 3), (2, 2), (3, 1)\}$

## Discrete Random Variables
$X$ is a discrete random variable, if its range is countable
## Probability Mass Function
Let $X$ be a discrete random variable with range $R_X = \{x_1, x_2, x_3, \dots\}$. The function:
$$\Large
\boxed{P_{X}(x_k) = P(X = x_k), \text{ for k } = 1, 2, 3, \dots}
$$
is called the probability mass function

> ***The PMF is a probability measure that gives us probabilities of the possible values for a randome variable***

- For discrete random variables, the PMF is also called the probability distribution
### Properties of PMF:
$$
\Large
\boxed{
\begin{aligned}
&0 \leq P_{X}(x) \leq 1 \text{ for all } x \\
&\sum_{x \in R_X} P_{X}(x) = 1 \\
&\text{for any set } A \subset R_X, P(X \in A) = \sum_{x \in A}P_{X}(x)
\end{aligned}
}
$$
### Examples
 - We draw 5 times out of a box with replacement (10 whites and 6 blacks). Let $X$ be the number of whites. What is PMF of $X$
	 - $R_X = \{0,1,\dots,5\}$
	 - $P(X = x_k) = {5 \choose x_k}(P(whites))^{x_k}(P(blacks))^{5 - x_k}$ 

### Short form Representations:
- $X \sim Unif(\Omega)$: $X$ is uniformly distributed over the set $\Omega$
- $X \sim H_{n, w, s}$: $X$ follows hypergeometric distribution
	- $n$: number of draws
	- $w$: number of "white"  objects
	- $s$: number of "black" objects
- $X \sim B_{n, p}$: $X$ follows binomial distribution
	- $n$: number of independent trials
	- $p$: probability of success per trial
- $X \sim G_p$: $X$ follows geometric distribution
	- $p$: probability of success per trial
- $X \sim P_{\lambda}$: $X$ follows Poisson distribution
	- $\lambda$ : expected rate of occurences of a event over given period of time

## Cumulative Distribution Function
- advantage: can be defined for any kind of random variable (discrete, continuous, mixed)
### Definition
- The cumulative distribution function (CDF) of random variable $X$ is defined as
$$\Large
\boxed{F_{X}(x) = P(X \leq x), \text{ for all } x \in \mathbb{R}}
$$
- the CDF stays flat between $x_k$ and $x_{k+1}$:
$$
F_{X}(x) = F_{X}(x_k), \text{ for } x_k \leq x < x_{k+1}
$$
- The CDF jumps at each $x_k$:
$$
F_{X}(x_k) - F_{X}(x_k - \varepsilon) = P_{X}(x_k)
$$
	- The size of the jump at each point is equal  to the probability at that point
- The CDF is always a non-decreasing function $\Leftrightarrow$ if $y \geq x$ , then $F_{X}(x) \geq F_{X}(y)$ 
- The CDF approaches 1 as x becomes large:
$$\Large
\boxed{\lim_{x \to \infty} F_{X}(x) = 1}
$$

![[WI - CDF for a discrete random variable.png]]

### Useful formula
- For all $a \leq b$, we have 
$$\Large
\boxed{
	P(a < X \leq b) = F_{X}(b) - F_{X}(a)
}
$$
Proof:
- We have $a \leq b \rightarrow P(X \leq b) = P(X \leq a) + P(a < X \leq b) \Leftrightarrow F(X \leq b) = F(X \leq a) + P(a < X \leq b)$   
## Example
- Tossing a coin twice. Let $X$ be the number of observed heads. Find the CDF of $X$
- Note that here $X \sim B(2, \frac{1}{2})$. The range of $X$ is $R_X = \{0, 1, 2\}$ and its PMF is given by:
	- $P_{X}(0) = P(X = 0) = \frac{1}{4}$
	- $P_{X}(1) = P(X = 1) = \frac{1}{2}$
	- $P_X(2) = P(X = 2) = \frac{1}{4}$

- Finding CNF, we argue as follows
- If $x < 0$, then $F_{X}(x) = P(X \leq x) = 0, \text{ for x } < 0$
- If $x \geq 2$, then $F_{X}(x) = P(X \leq x) = 1, \text{ for x } \geq 2$
- If $0 \leq x < 1$, then $F_{X}(x) = P(X \geq x) = P(X = 0) = \frac{1}{4}$, for $0 \leq x < 1$
- If $1 \leq x < 2$, then $F_{X}(x) = P(X = 0) + P(X = 1) = \frac{1}{4} + \frac{1}{2} = \frac{3}{4}$, for $1 \leq x < 2$

> ***At point $x =1$ , the CDF jumps from $\frac{1}{4}$ to $\frac{3}{4}$. The size of the jump is $\frac{3}{4} - \frac{1}{4} = \frac{1}{2}$ ***
> ***Also note that the open and closed circles at point $x =1$ indicate that $F_{X}(1) = \frac{3}{4} \text{ not } \frac{1}{4}$***

![[CNF example.png]]

## Continuous Random Variable

