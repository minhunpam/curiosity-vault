## State Diagram
-  Denote states with circles and give them symbolic names
- Define one of the states as the initial state
	- Before the first rising clock edge, there is the initial period, in which the FSM  is in the "initial state"
- With arrows we define the sequnece of states
- The sequence of states can also be defined in a state transistion table

| present state | next state |
| ------------- | ---------- |
| A             | B          |
| B             | C          |
| C             | A          |
- FSMs typically also have inputs influencing the transisiton to the nexts state (`next_state = f(current_state, input)`)
	- For example:
		- `A = f(B, 0)`
		- `C = f(B, 1)`

	- The following state can also be the same as the current state (aka. a loop)
	- At each rising clock edge, the `next_state` is computed and loaded by the flip-flops
	- Between edges, the state signal stays constant. Changes on the `input` during a clock cycle don't change the state until the next edge

- FSMs typically also have outputs
	- We write the output into the circles
	- We call such machines also [[#Moore Machine]]
	- We define the outputs with the "output function": `output = f(state)`


| state | output |
| ----- | ------ |
| A     | 4      |
| B     | 3      |
| C     | 2      |
## Mapping a State Diagram to Hardware
### Moore Machine
![[CON - Moore Machine.png]]
#### State Encoding
- We use binary encoding (e.g: We need 2 bits to encode the 3 states A, B, C)

| State | Encoding |
| ----- | -------- |
| A     | 00       |
| B     | 01       |
| C     | 10       |
#### State Transition Table

| present state | in  | next state |
| ------------- | --- | ---------- |
| A             | 0   | B          |
| A             | 1   | B          |
| B             | 0   | A          |
| B             | 1   | C          |
| C             | 0   | A          |
| C             | 1   | C          |

| present |        | in  | next   |        |
| ------- | ------ | --- | ------ | ------ |
| **s1**  | **s0** |     | **s1** | **s0** |
| 0       | 0      | 0   | 0      | 1      |
| 0       | 0      | 1   | 0      | 1      |
| 0       | 1      | 0   | 0      | 0      |
| 0       | 1      | 1   | 1      | 0      |
| 1       | 0      | 0   | 0      | 0      |
| 1       | 0      | 1   | 1      | 0      |
| 1       | 1      | 0   | x      | x      |
| 1       | 1      | 1   | x      | x      |
> "11" doesn't exist so we mark "x" for: don't care

$$
\begin{aligned}
next\_s0 &= (\neg s1 \land \neg s0 \land \neg in) \lor (\neg s1 \land \neg s2 \land in) \\
next\_s1 &= (\neg s1 \land s0 \land in) \lor (s1 \land \neg s0 \land in)
\end{aligned}
$$
#### Output function

| State | s1  | s0  | Output | o2  | o1  | o0  |
| ----- | --- | --- | ------ | --- | --- | --- |
| A     | 0   | 0   | 4      | 1   | 0   | 0   |
| B     | 0   | 1   | 3      | 0   | 1   | 1   |
| C     | 1   | 0   | 2      | 0   | 1   | 0   |
$$
\begin{aligned}
o2 &= \neg s1 \land \neg s0 \\
o1 &= (\neg s1 \land s0) \lor (s1 \land \neg s0) \\
o0 &= \neg s1 \land s0
\end{aligned}
$$
#### Essence of Moore Machines
- Starting with the sequential logic in the state encoding step, where the state is stored in a register
	- With "areset", we can initialize the ASM ("initial state")
	- A state transition is cause by the clock signal at the rising clock edge
- With the next-state function $f(state, input) = next\_state$ , we can compute the next state with feeding input
- With the output function $g(state) = output$ , we compute the output values
### Mealy Machine
- next state = function of present state and input
- output = function of present state and input (a.k.a the output is dependent on the state and input)

![[CON - Mealy Machine.png]]

#### The output function

| state | in  | output |
| ----- | --- | ------ |
| A     | 0   | 4      |
| A     | 1   | 4      |
| B     | 0   | 3      |
| B     | 1   | 1      |
| C     | 0   | 2      |
| C     | 1   | 0      |
#### Timing Diagram
![[CON - TIming diagram for Mealy Machine.png]]
>***Note how the value if "in" immediately influences the value of "out"***

## Data path & Control Unit
### Data Path
- contains everything that is related to data processing
	- **consists of**:
		- functional units doing computations
		- data registers
		- wires and multiplexers connecting the registers and functional units
	- **takes as inputs**:
		- control signals defining "which computations are performed and where data is stored"
	- **provides as outputs**:
		- data values and derived values
$\rightarrow$ **Often visualized based on tblock diagrams; state diagrams are not suitable**

#### Data Path in SystemVerilog
##### 1. Why do we need set default values for registers in `always_comb` block?
```SystemVerilog
always_comb begin
	// default settings for all registers
	x_n = x_p;
	y_n = y_p;
	
	// ...
end
```
- Without those defaults, the synthesizer would detact that:
	- In some control signal combinations, `x_n` or `y_n` are not assigned in `always_comb` block. Whereas in hardware, every signal must always have  a defined value at all times
	- If not, the synthesizer will assume, that ***The circuit needs to remember the previous value whe not assigned*** and it will ***infer a latch***
### Control Units
 - A Control Unit/ Controller/ FSM "contains everything is related managing what happens what happens when and under what condition"
	 - **consists of**:
		 - next-state logic
		 - state registers
		 - wires connecting the next-state logic and registers
	- **takes as inputs**:
		- control signals and status flags (system conditions)
	- **provides as outputs**:
		- control signals connected to a datapath taking them as input
$\rightarrow$ **Best visualized based on state diagrams**
## Algorithmic State Machines (Modelling control and Data Path in a Single Graph)
> All FSM state diagrams have an equivalent ASM diagram

### Adding the Data path using _register-transfer-statements_
- _register-transfer statements_ define the change of a value stored in register of the data path
	- Values in registers can only change at the rising clock edge
	- "Register-transfer statements" with a "left arrow" ("$\leftarrow$")
		- Example: "$a \leftarrow x$" means that the value in the register `a` gets the value of "x" at the "next" rising clock edge

#### "=" versus "$\leftarrow$" in a ASM

| ASM          | SystemVerilog | Meaning                                                                                                                |
| ------------ | ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| =            | =             | denotes the output of the FSM has a certain value during a particular state                                            |
| $\leftarrow$ | <=            | The register value left of the arrow changes to whatever is defined right of the arrow upon the next rising clock edge |
## Control and Datapath in SystemVerilog

|                 | Control Unit                  | Data Path                                       |
| --------------- | ----------------------------- | ----------------------------------------------- |
| **always_comb** | describes next-state logic    | describes the functional units of the data path |
| **always_ff**   | describes the state registers | describes the data registers                    |

