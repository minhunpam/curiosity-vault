## Big Oh Notation & Small-o notation
- $$\Large \mathcal{O}(g(n)) = \{f: \mathbb{N} \to \mathbb{R} | \exists c \in \mathbb{R}^{+}, n_0 \in \mathbb{N}: 0 \leq f(n) \leq c \cdot g(n), \forall n \geq n_0    \}$$   
![[Big O.png]]
- $$\Large f(n) = o(g(n)) \Leftrightarrow \forall c > 0, \exists n_0 >0: \forall n \geq n_0, f(n) < cg(n) \Leftrightarrow \lim_{n\to\infty}\frac{f(n)}{g(n)} = 0$$
	- $f(n)$ grows stricly slower than $g(n)$
	- No matter how small a constant $c$ you choose, $f(n)$ eventually becomes **smaller** thatn $c\cdot g(n)$ -> There is no fixed constant $c$ that can "tightly" upper-bound $f(n)$ in the long run
## Big Omega Notation
- $$\Large \Omega(g(n)) = \{f: \mathbb{N} \to \mathbb{R} | \exists c \in \mathbb{R}^{+}, n_0 \in \mathbb{N}: 0 \leq c \cdot g(n) \leq f(n), \forall n \geq n_0 \}$$
![[Big Omega.png]]
- $$\Large f(n) = \omega(g(n)) \Leftrightarrow \forall c > 0, \exists n_0 > 0: \forall n \geq n_0, f(n) > cg(n) \Leftrightarrow \lim_{n\to\infty}\frac{f(n)}{g(n)} = \infty$$
	- $f(n)$ grows stricly faster than $g(n)$
## Big Theta Notation
- $\Large\Theta(g(n)) = \{f: \mathbb{N} \to \mathbb{R} | \exists c_1, c_2 \in \mathbb{R}^{+}, n_0 \in \mathbb{N}: 0 \leq c_1 \cdot g(n) \leq f(n) \leq c_2 \cdot g(n), \forall n \geq n_0    \}$ 
	- $\Large\Theta(g(n)) = \mathcal{O}(g(n)) \cap \Omega(g(n))$
![[Big Theta.png]]
## Limits
$$
\Large
\lim_{n \to \infty} \frac{f(n)}{g(n)} = 
\begin{cases}
0 &f(n) = \mathcal{O}(g(n)) \\
0 < c < \infty &f(n) = 
	\begin{cases}
		\Theta(g(n)): \text{they grow at the same asymtotic rate} \\
		\mathcal{O(g(n))}: f(n) \textbf{ grows no faster than } g(n) \\
		\Omega(g(n)): f(n) \textbf{grows no slower than } g(n)
	\end{cases} \\
\infty &f(n) = \Omega(g(n))
\end{cases}
$$
$$
\Large
\begin{cases}
	\liminf_{n \to \infty} \frac{f(n)}{g(n)} > 0 &f(n) = \Omega(g(n)) \\
	\limsup_{n \to \infty} \frac{f(n)}{g(n)} > 0 &f(n) = \mathcal{O}(g(n))
\end{cases}
\quad \Rightarrow \text{If both are true}: f(n) = \Theta(g(n))
$$
## Computing with asymtotic notation
### Addition
- $\Theta(f(n)) + \Theta(g(n)) = \Theta(max{f(n), g(n)})$
- $\Sigma_{i} \Theta(f_i(n)) = \Theta(\Sigma_{i} f_i(n))$

### Multiplication
- $c \cdot \Theta(f(n)) = \Theta(f(n)) \text{ for constant } c > 0$
- $\Theta(f(n)) \cdot \Theta(g(n)) = \Theta(f(n) \cdot g(n))$
