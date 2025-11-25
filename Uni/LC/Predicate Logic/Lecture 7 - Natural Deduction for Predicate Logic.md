## Proof Rules for Universal Quantification
$$
\Large
\boxed{
\frac{\forall x \varphi}{\varphi [t/x]}
\forall_e
}
$$
- $\forall x \varphi$ is true, we are allowed to replace the $x$  in $\varphi$ with any term $t$
- Substitution $[t/x]$ 
	- $t$ - Term
	- $x$ - Variable
	- Reads: "$\varphi$ with $t$ for $x$"
- Example:
$$
\Large
\frac{
	\forall x (P(f(x, y)) \lor Q(x))
}{
	P(f(a, y) \lor Q(a))
}
\forall e \quad \text{with } \varphi[a/x]
$$
### Conditions for Substitution
1. Replace only free variables
$$

$$


## Proof Rules for existential introduction
$$
\Large
\boxed{
\frac{\varphi [t/x]}{\exists x \varphi}\exists_i
}
$$
- If $\varphi$ holds for some term $t$, we can conclude that there $\exists x$ such that $\varphi$ holds
- Side condition: $t$ must be **free** for $x$ in $\varphi$
### Example: $\forall (P(x) \to Q(y)), \forall y(P(y) \lor R(x)) \vdash \exists x Q(x)$
$$
\begin{aligned}
&1.\forall (P(x) \to Q(y)) &\quad prem. \\
&2.\forall (P(y) \land R(x)) &\quad prem. \\
&3. \forall (P(t) \to Q(y)) &\quad \forall e 1 \\
&4. P(t) \land R(x) &\quad \forall e2 \\
&5. P(t) &\quad \land e_1 4 \\
&6. Q(y) &\quad \to e 3,5 \\
&7. \exists x Q(x) &\quad \exists i 6
\end{aligned}
$$
## Proof rules for Existential Quantification
$$
\Large
\boxed{
\frac{
\exists x \varphi \quad 
\boxed{
\begin{array}{c}
	x_0 \\
    \varphi[x_0/x] \quad ass. \\
    \vdots \\
    \mathcal{X}
    \end{array}
}
\quad
x_0 \text{ fresh}
}{\mathcal{X}}\exists_e
}
$$

- From $\exists x \varphi$, we know that $\varphi$ is true for at least one value of $x$
- If we can prove $\mathcal{X}$ without the exact knowledge of the value x, then $\mathcal{X}$ can be deduced simply from the fact that there exists a value for $x$
	- Thus we use a fresh variable $x_0$ in the proof
	- If by assuming $\varphi[x_0/x]$, we can prove $\mathcal{X}$ inside the box, the $\mathcal{X}$ can be deduced outside the box
-


## Invalid Sequents
- To prove that a sequent is invalid, we need to find a model $\mathcal{M}$ that is counterexample
- $\mathcal{M}$ is a counterexample, if...
	- $\mathcal{M}$ satisfies **all premises**, and
	- $\mathcal{M}$ doesn't satisfy the **conclusion**



