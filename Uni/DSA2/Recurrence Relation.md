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
There are 3 cases, but all 3 are disjoints. That means, if one case is true, the others are false
- Case 1: $f(n) = \mathcal{O}(n^{\log_b a - \epsilon}) \text{ for some } \epsilon > 0 \Rightarrow T(n) = \Theta(n^{log_b a})$ 
- Case 2: $f(n) = \Theta(n^{log_b a}) \Rightarrow T(n) = \Theta(n^{log_b a}log n)$
- Case 3: $f(n) = \Omega(n^{log_b + \epsilon})$
### Reasons not to use the Master Method:
1. Recurrence form doesn't hold
2. Properties of each case don't hold

