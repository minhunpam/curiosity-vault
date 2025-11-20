## Definition
- equation (inequality) that describes a function in terms of its values on smaller inputs, plus one or more initial terms
- Example:
	- Factorial function: $f(n) = n \cdot f(n-1) \text{ for } n > 1, f(1) = 1$
	- Fibonacci function: $f(n) = f(n-1) + f(n-2) \text{ for } n > 2, f(1) = f(2) = 1$
n

- Methods to solve recurrence relations
1. The iterative method
2. The master method
3. The substitution method

## 1. The iterative method
- Idea: repeadtedly plug in recurrence
- The runtime for this method is $T(n) = \Theta(n^2)$
- Note:
![[sum of asymptotic values rule.png]]
- The point of this method is at some point when we reach the general form of the expression. We would like to find a $k$ such that turning the $T(\dots)$ to a $const$, doesn't matter what value of that is, but I have consider carefully if the value 0 is valid
	- Ideally, the value would be 1
- For example: $T(\frac{n}{2^k})$  - in this case we will not choose 0 to be the $const$ because otherwise $k \to \infty$ . In contrary, when this expressions becomes small enough, we want to stop


## 2. Recusions trees
- Visualization of recursion

## 3. The master method
$$T(n) = aT(\frac{n}{b}) + f(n) \text{ with } a \geq 1 \wedge b > 1$$
where:
- $a$ represents the number of childeren each node has
- $T(\frac{n}{b})$ is the runtime of each of $a$ initial node (level 1 of the tree)
- Depth of the tree: $log_{b}{n}$
	- At depth $i$ contains $a^i$ nodes
	$\Rightarrow$ There are $a^{log_{b}{n}} = n^{log_{b}{a}}$ leaves $\Rightarrow$ the runtime is $\Theta(n^{log_{b}{a}})$ 
	
> The master theorem argues that, it is possible to determine the asymtotic form of $T$ based on the **relative comparison between f and $n^{log_{b}{a}}$**

There are 3 cases, but all 3 are disjoints. That means, if one case is true, the others are false
- **CASE 1: Recursive Part dominates**  
 $$
 \begin{aligned}
 & \Large f(n) = \mathcal{O}(n^{\log_b a - \varepsilon}) \text{ for some } \varepsilon > 0 \\ 
 & \Large \Rightarrow T(n) = \Theta(n^{log_b a})
 \end{aligned}
$$ 
- **CASE 2: None grows polynomially faster than the other**
$$
\begin{aligned}
&\Large f(n) = \Theta(n^{log_b a}log^{k}{n}), \text{where k } \geq 0\\
&\Large \Rightarrow T(n) = \Theta(n^{log_b a}log^{k +1}n)
\end{aligned}
$$
> Here the `log` term accounts for the logarithmic contribution of each recursion level

- **CASE 3: Non-recursive Part dominates** 
$$
\begin{aligned}
&\large f(n) = \Omega(n^{log_b + \varepsilon}) \text{ for some } \varepsilon > 0 \land \exists c < 1 : a \cdot f(\frac{n}{b}) \leq c \cdot f(n), n \geq n_0 \\ 
&\Large \Rightarrow T(n) = \Theta(f(n))
\end{aligned}
$$

### Examples
1. $T(n) = 4T(\frac{n}{2}) + \Theta(n)$
	- $a = 4 \geq 1$
	- $b = 2 > 1$
	- $f(n) = \Theta(n)$
	Compare $f(n)$ with $n^{log_{b}{a}}$:
	$$
	\begin{aligned}
	f(n) &\stackrel{\text{?}}{=} \mathcal{O}(n^{2-\varepsilon}) \text{ for } 2-\varepsilon \geq 1 \Leftrightarrow 1 \geq \varepsilon > 0 \rightarrow \varepsilon = 1 \rightarrow \text{YES}\\
	&\stackrel{\text{?}}{=} \Theta(n^2) \rightarrow \text{NO} \\
	&\stackrel{\text{?}}{=} \Omega(n^{2 + \varepsilon}) \text{ for } 2 + \varepsilon \leq 1 \Rightarrow \varepsilon \leq -1 \rightarrow \text{NO}\\
	&\Rightarrow T(n) = \Theta(n^{log_{b}{a}}) = \Theta(n^2)
	\end{aligned}
	$$

2. $T(n) = 4T(\frac{n}{2}) + \Theta(n^2)$
	$$
	\begin{rcases}
	a = 4 \geq 1\\
	b = 2 > 1
	\end{rcases}
	\rightarrow \log_{b}{a} = log_{2}{4} = 2 \rightarrow n^{log_{b}{a}} = n^2
	$$
	- $f(n) = \Theta(n^2)$
	Compare $f(n)$ with $n^{log_{b}{a}}$
	$$
	\begin{aligned}
	f(n) = \Theta(n^2) &\stackrel{\text{?}}{=} \mathcal{O}(n^{2-\varepsilon}) \text{ for } 2-\varepsilon \geq 2 \rightarrow \varepsilon \leq 0 \rightarrow \text{NO}\\
	&\stackrel{\text{?}}{=} \Theta(n^2) \rightarrow \text{YES} \\
	&\stackrel{\text{?}}{=} \Omega(n^{2 + \varepsilon}) \text{ for } 2 + \varepsilon \leq 2 \Rightarrow \varepsilon \leq 0 \rightarrow \text{NO} \\
	&\Rightarrow T(n) = \Theta(n^{log_{b}{a}}logn) = \Theta(n^2logn)
	\end{aligned}
	$$
3. $T(n) = 3T(\frac{n}{2}) + \Theta(n^2)$
	$$
	\begin{rcases}
	a = 3 \geq 1\\
	b = 2 > 1
	\end{rcases}
	\rightarrow \log_{b}{a} = log_{2}{3} \in (1, 2) \rightarrow n^{log_{b}{a}} = n^{log_{2}{3}}
	$$
	- Compare $f(n)$ with $n^{log_{a}{b}}$
		$$
		\begin{aligned}
		f(n) = \Theta(n^2) &\stackrel{\text{?}}{=} \mathcal{O}(n^{\log_{2}{3}-\varepsilon}) \text{ for } \log_{2}{3}-\varepsilon \geq 1 \rightarrow \varepsilon \leq 1 - \log_{2}{3} \in (0, 1) \rightarrow \text{NO}\\
		&\stackrel{\text{?}}{=} \Theta(n^{\log_{2}{3}}) \rightarrow \text{NO} \\
		&\stackrel{\text{?}}{=} \Omega(n^{\log_{2}{3} + \varepsilon}) \text{ for } \log_{2}{3} + \varepsilon \leq 2 \Rightarrow \varepsilon \leq 2 - \log_{2}{3} \in (0, 1) \text{but we have to check additional condition}\\
		\end{aligned}
		$$

	- Check additional condition:
	$$
	\begin{aligned}
	&\exists c < 1 s.t.: af(\frac{n}{b}) \leq c \cdot f(n) \text{ for } n \geq n_o \\
	& af(\frac{n}{b}) = 3\cdot (\frac{n}{2})^2 = \frac{3}{4}n^2 \leq c\cdot n^2 \\
	& \rightarrow \text{YES} : \text{for } n \geq 1 \land \frac{3}{4} \leq c < 1 \\
	& \Rightarrow T(n) = \Theta(f(n)) = \Theta(n^2)
	\end{aligned}
	$$
	



### Reasons not to use the Master Method:
1. Recurrence form doesn't hold
2. Properties of each case don't hold
	- Consider an example: $T(n) = 8T(\frac{n}{2}) + \frac{n^2}{log n}$ 
		- $a = 8, b = 2 \rightarrow n^{\log_{b}{a}} = n^3$
		- $f(n) = \frac{n^3}{log n}$ 
	- Compare $f(n)$ with $n^{log_{b}{a}}$
		- $f(n)$ is smaller than  $n^3$ - but only by the factor $\frac{1}{log n}$
	- A polynomial factor means something like $n^{\varepsilon}$ for some constants $\varepsilon > 0$
		- If $f(n) = \frac{n^3}{n^{\varepsilon}} = n^{3 - \varepsilon}$ , then $f(n)$ is smaller than $n^3$ **by a polynomial factor**. But here, the difference is only $logn$ which grows slower than **any polynomial**
	- $\Rightarrow$ it lies between **Case 1 and Case 2** $\rightarrow$ Master theorem can't directly handle it
## 4. The substitution method
- Idea: "guess" an asymtotic bound $(\mathcal{O}, \Omega)$ and prove it by mathematical induction
- Example: $T(n) = 2T(\lfloor\frac{n}{2}\rfloor) + n$
	- Guess: $T(n) = \mathcal{O}(nlogn)$
	- We have to show that: $\exists c > 0, n_0 \in \mathbb{N}$ such that $T(n) \leq c\cdot nlogn$ for all $n \geq n_0$   
		- **Induction Base:** $T(k) = \Theta(1) \leq d$ for small constant $k$ and some constant $d > 0$
		- **Induction Hypothesis:** $T(n) \leq c\cdot nlog_{2}{n}$ for some const $c > 0$
		- **Induction Step:** $T(n) = T(\lfloor\frac{n}{2}\rfloor) + n \leq 2\cdot (c\cdot \lfloor \frac{n}{2} \rfloor log_{2}{\lfloor \frac{n}{2} \rfloor}) + n \stackrel{\text{?}}{\leq} c\cdot log_{2}{n}$   
			$$\begin{aligned}
	T(n) = T(\lfloor\frac{n}{2}\rfloor) + n &\leq 2\cdot (c\cdot \lfloor \frac{n}{2} \rfloor log_{2}{\lfloor \frac{n}{2} \rfloor}) + n \\
	&\leq c\cdot nlog_{2}{\frac{n}{2}} + n \\
	& = c \cdot nlog_{2}{n} - c \cdot nlog_{2}{2} + n \\
	& = c \cdot nlog_{2}{n} - cn +n \\
	& = c \cdot nlog_{2}{n} + (1-c)n \\
	& \leq c \cdot nlog_{2}{n} \quad \text{ for } c \geq 1 \\
	&\text{Choose $k \geq 3$ for induction base, $c \geq max\{1, d\}, n_0 \geq 2$}
\end{aligned}$$
### Attention:
1. ***Don't use asymptotic notation in the induction step***
2. ***Sometimes the "obvious" induction hypothesis doesn't work**
3. ***How to make right guess?***
	- We can rely on the the recurrence relation when it is resolved in 1 or 2 first steps and it recurssion tree
		- We need to know the non-recursive work at each level
		- We need to know the depth of the tree
		$\Rightarrow \text{total work = non-recursive work} \times \text{depth of the tree}$ 
		
		
		
		
		
		
		
		
