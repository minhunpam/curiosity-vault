## Goal:
- Understand the concept of a finite state machine
- implement Stein's algorithm (Binary GCD Algorithm) as Register-Transfer-Level (RTL) model
- Tasks
	- derive an ASM diagram of a state machine
	- implement the binary GCD in hardware description language $\texttt{SystemVerilog}$

## 1. Binary GCD Algorithm
- computes the greatest common divisor (GCD) for 2 **non-negative integers**
1. Checks whether 1 of the input values (or both) are zero and return the according GCD value
	1. $\texttt{gcd(a, 0) = a}$
	2. $\texttt{gcd(0, b) = b}$
	3. $\texttt{gcd(0, 0) = 0}$
2. Calculates $2^k$ - the greatest power of 2 value that divides a and b
	- This step continues under the condition: both a and b are even
	- In each iteration, it removes all common powers of 2 from `a` and `b` and counts how many such factors there are
	- `k` stores the number of factors of 2 `a` and `b` had in common
3. The algorithm divides `a` by 2 using RIGHT shift operations until a becomes an odd number. Same for b afterwards
	- Depending on the values of a and b (`if a > b`), the values potentially get swapped
	- And b is reassigned to `b - a`
	- The process of division, potential swapping, and subtraction is repeated until b is zero
4. Calculate GCD value using $\texttt{a}$ by LEFT shifting the computed result by $\texttt{k}$
	$$
	gcd(a,b) = a << k
	$$

## 2. Specification
### Derivation of ASM diagram from the pseudocode
1. Divide the pseudocode into different states
2. Desgin the datapath based on the states of the ASM diagram
3. Add register transfer statements and outputs for each state
4. Add the control logic for all state transitions and condition checks to the ASM diagram
## 3. What did I do?
- I divided the program into separate parts: 
	- The control unit
	- The data path
- Each single part has 
	- 1 `always_ff` block
	- 1 `always_comb` block
### Control Unit
- I introduced 
	- an enum of 3-bit states (maximum 8 states)
	- control signals on which the data path can rely to do the proper operations
- `always_ff`
	- on reset -> go to `INIT`
	- on every clock cycle -> move from `state_p` to `state_p`
- `always_comb`
	- Has default assignments for each control signal
	- In each state, it will check the value of control signals to determine
		- the change of some control signals
		- what the next state is

### Datapath
- I introduced different 32-bit-registers for `a`, `b` and `k`
- `always_ff`
	- on reset -> set the 'present' registers
	- on `start_i` and `state_p == INIT` -> reset the 'present' registers
- `always_comb`
	- Do operations with respect to matching control signals