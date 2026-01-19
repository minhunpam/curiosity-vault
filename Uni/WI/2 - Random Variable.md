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
## Probability Mass Function (PMF)
Let $X$ be a discrete random variable with range $R_X = \{x_1, x_2, x_3, \dots\}$. The function:
$$\Large
\boxed{P_{X}(x_k) = P(X = x_k), \text{ for k } = 1, 2, 3, \dots}
$$
is called the probability mass function

> ***The PMF is a probability measure that gives us probabilities of the possible values for a random variable***

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

## Cumulative Distribution Function (CDF)
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
	- **The size of the jump at each point is equal  to the probability at that point**
- The CDF is **always a non-decreasing function** $\Leftrightarrow$ if $y \geq x$ , then $F_{X}(z) \geq F_{X}(x)$ 
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
### Expectation of discrete random variable
Let $X$ be a discrete random variable with range $R_X = \{x_1, x_2, x_3, \dots\}$ (finite or countably infinite). The ** expected value/ mean** of $X$, denoted by $E(X)$, is defined as:
$$\Large
\boxed{
  E(X) = \sum_{x_k \in R_X}x_kP(X=x_k) = \sum_{x_k \in R_X}x_kP_X(x_k)
}
$$
- Different notations:
$$\Large
\boxed{
  E(X) = EX = E[X] = \mu_X
}
$$
#### Example:
- So I define a discrete random variable: $X = \text{Number of workouts in a week}$ and I have a probability distribution as following:

| $X$ | $P(X)$ |
| --- | ------ |
| 0   | 0.1    |
| 1   | 0.15   |
| 2   | 0.4    |
| 3   | 0.25   |
| 4   | 0.1    |
- The expected value that I calculated is: $2.1$ but the question is what 0.1 workout. Well this might not be true when saying I do 2.1 workouts in a single week. But this value is valuable for a long run, says after 10 weeks, the expected number of workouts is 21 times
#### Linearity of expectation
- If $X_1, X_2, \dots , X_n$ are random variables and $a_1, a_2, \dots, a_n$ are constants, then:
$$\Large
\boxed{
  E(\sum_{i=1}^n a_i X_i) = \sum_{i=1}^n a_i E[X_i] 
}
$$
- In particular, $E[X + Y] = E[X] + E[Y]$ 

### Variance of discrete random variable
- The **variance** is the measure of how spread out the distribution of a random variable is. The **variance** of a random variable $X$, with mean $EX = \mu_{X}$, is defined as
$$
\boxed{
  Var(X) = E[(X - \mu_{X})^2]
}
$$
- A large value of variance means that the distribution is very spread out.
- A low value of variance means that the distribution is concentrated around its average
> **Note**: $Var(X)$ has a different unit than $X$. For example, if $X$ is measure  in $meters$ then $Var(X)$ is in $meters^2$ . To solve this issue, we define another measure, called the **stardard deviation**, usually shown as $\sigma_{X}$ , which is simply the square root of variance

$$
\boxed{
  SD(X) = \sigma_{X} = \sqrt{Var(X)}
}
$$
#### Useful formula for computing the variance
$$
\boxed{
	Var(X) = E[X^2] - [EX]^2
}
$$
##### Example:
- Roll a fair dice and let $X$ be the resulting number. Find $E(X), Var(X), \sigma_X$
- We have $R_X = \{1,2,3,4,5,6\}$ and $P_{X}(k) = \frac{1}{6}$ for $k=1,2,\dots,6$. Thus we have:
	- $EX = 1.1/6 + 2.1/6 + 3.1/6 + 4.1/6 + 5.1/6 + 6.1/6 = 7/2$
	- $EX^2 = 1.1/6 + 4.1/6 + 9.1/6 + 16.1/6 + 25.1/6 + 36.1/6 = 91/6$
	---> $Var(X) = EX^2 - (EX)^2 = 91/6 - (7/2)^2 \approx 2.92$
	---> $\sigma_X = \sqrt{Var(X)} \approx \sqrt{2.92} \approx 1.71$
	 
#### Theorems:
1. For a random variable $X$ and real numbers $a$ and $b$
$$
\boxed{
	Var(aX + b) = a^2Var(X)
}
$$
Hence, we can conclude, for standard deviation, $SD(aX + b) = |a|SD(X)$
2. **If $X_1, X_2, \dots, X_n$ are independent random variables and $X = X_1 + X_2 + \dots + X_n$, then:**
$$
\boxed{
	Var(X) = Var(X_1) + Var(X_2) + \dots + Var(X_n)
}
$$


## Continuous Random Variable
- A random variable $X$ with CDF $F_X(x)$ is said to be continuous if $F_X(x)$ is a continuous function for all $x \in \mathbb{R}$.
> **We will also assume that the CDF of continuous random variable is differentiable almost everywhere in $\mathbb{R}$**
## Probability Density Function (PDF)
- The PMF does not work for continuous random variables, because for a continuous random variable $P(X=x)=0$ for all $x \in \mathbb{R}$
- **Consider a continuous random variable $X$ with absolutely continuous CDF $F_X(x)$. The function $f_X(x)$ defined by:**
$$\Large
\boxed{
f_X(x) = \frac{dF_X(x)}{dx} = F'_{X}(x), \text{if $F_X(x)$ is differentiable at x}
}
$$
is called **Probability Density Function (PDF) of $X$**
- Since PDF is the derivate of the CDF, the CDF can be obtained from PDF by integration (assuming absolute):
$$\large
F_X(x) = \int_{-\infty}^{x}f_X(u)du
$$
Also, we have:
$$\large
P(a < X \leq b) = F_X(b) - F_X(a) = \int_{b}^{a}f_X(u)du
$$
In particular, if we integrate over the entire real line, we must get 1:
$$\large
\int_{-\infty}^{\infty}f_X(u)du = 1
$$
### Properties of the PDF:
1. $f_X(x) \geq 0 \text{ for all } x \in \mathbb{R}$
2. $\int_{-\infty}^{\infty}f_X(u)du = 1$
3. $P(a < X \leq b) = F_X(b) - F_X(a) = \int_{b}^{a}f_X(u)du$
4. More generally, for a set $A$, $P(X \in A) = \int_{A}f_X(u)du$
	- An example of set $A$ could be an union of some disjoint intervals.
	- For example: if you want to find $P(X \in [0, 1] \cup [3, 4]$, you can write: $P(X \in [0, 1] \cup [3, 4]) = \int_{0}^{1}f_X(u)du + \int_{3}^{4}f_X(u)du$


### Key properties of a joint density:
1. It must be non-negative
$$
f(x,y) \geq 0
$$
2. The total probability over the entire region is 1
$$
	\int{\int f(x, y) dx}dy =1
$$
3. To get probabilities, you integrate
$$
P(a \leq X \leq b, c \leq Y \leq d) = \int_{a}^{b} \int_{c}^{d} f(x,y) dy dx
$$
### The marginal densities
- Marginal of X:
$$
f_X(x) = \int_{0}^{1}f(x,y)dy
$$
- Marginal of Y:
$$
f_Y(x) = \int_{0}^{1}f(x,y)dx
$$

## Normal distribution (Gaussian distribution)
- A type of continuous probability distribution for a real-valued random variable
- Th general form of its probability density function is
$$\Large
f(x) = \frac{1}{\sqrt{2\pi \sigma^2}}e^{-\frac{(x - \mu)^2}{2 \sigma^2}}
$$
	-   $\mu$ ...mean/ expectation of the distribution
	- $\sigma^2$ ...variance
	- $\sigma$ ... standard deviation

## Exponential Distribution
- It is often used to model the time elapsed between events
### Definition
- A continuous random variable $X$ is said to have an _exponential_ distribution with parameter $λ>0$, shown as $X∼Exponential(\lambda)$, if its PDF is given by:
$$\Large
f_X(x) =
\begin{cases}
\lambda e^{-\lambda x} &x > 0 \\
0 &\text{otherwise}
\end{cases}
$$
## Expectation
- Expected value = mean = average
- Let $X$ be discrete random variable with range $R_X = \{x_1, x_2, x_3, \dots\}$ (finite or countably infinite). The expected value of $X$

## Variance