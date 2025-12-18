## Difference between `Distinct` and `!=`
1. `!=` (binary inequality)
	- Works on 2 expressions only
	- Must be written pairwise for multiple variables
	- Simple and explicit
2. `Disctinct`(n-ary all-diffrent constraint)
	```python
	s.add(Distint(a, b, c))
	```
	- “Find a model in which **the values assigned to `a`, `b`, and `c` are all different**.”
	- 