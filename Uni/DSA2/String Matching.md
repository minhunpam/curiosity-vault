- Input: 
	- Text/ String T (typically a long text), length n
	- Pattern P (typically short), length $m << n$
- Goal: 
	- Find all occurrences of P in T
## Naive algorithm:
- Iterate through the text from left to right. The pattern can occur at position $s = 0, \dots, n -m$
- Check at each position whether the pattern fits by checking letter by letter
- Pseudocode:
```
TestPosition(s):
	t = 0
	while t < m and T[s+t] == P[t] do
		t = t + 1
	return (t==m)
```
- This algorithm would be put within a for loop running from 0 to n-m
- Runtime:
$$
\#(Iterations) =

\begin{cases}
\begin{aligned}
m \\
1 + min_{0 <i<m}
\end{aligned}	
\end{cases}

$$