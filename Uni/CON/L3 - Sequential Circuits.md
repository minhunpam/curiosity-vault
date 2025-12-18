## What is Sequential Circuit?
- **digital circuit that stores and uses previous state information to determine their next state**
- **sequential circuits depend on both the current inputs and the previous state stored in memory elements**

## A Simple Set-Reset Latch (NOR Version)

### What is a latch?
- is the **simplest memory element** in digital circuits
- is a small circuit that can store one bit - either 0 or 1 - until you tell it to change
#### Transparent Latch
- it responds to the level change of input immediately
	- That means:
		- If the input is 0, then the output is also 1
		- And suddenly the input changes to 1, then the output is also 1
#### Gated Latch
- Similar to Transparent Latch, but Gated Latch has an enable input
	- When `enable = 1`, latch responds to the input immediately (just like transparent latch). This behavior makes this latch to be called: ***level sensitive/triggered memory element***
	- When `enable = 0`, latch doesn't respond to the input change, but retain the past input value

![[CON - Gated Latch Timing Diagram.png]]

### Example
![[CON - Set-Reset Latch.png]]
#### Default Configuration (`E = 0`)
```
R & E = 0
S & E = 0
```
- the cross-coupled NOR Gates are just feeding their outputs back to each other
```
Q = NOR(0, ~Q)
  = NOT(0 | ~Q)
  = NOT(0) & NOT(~Q)
  = 1 & Q
  = Q
~Q = NOR(0, Q)
   = NOT(0 | Q)
   = NOT(0) & NOT(Q)
   = 1 & ~Q
   = ~Q
```
- A **stable feedback loop**

#### Setting the output (S = 1, R = 0, E = 1)
- Q = 1
- Set E = 0 -> Q stays at 1 ([[L3 - Sequential Circuits#Default Configuration (`E = 0`)| in the default config again]])

#### Resetting the output (S = 0, R = 1, E = 1)
- Q = 0
- Set E = 0 -> Q stays at 0 ([[L3 - Sequential Circuits#Default Configuration (`E = 0`)| in the default config again]])

#### Illegal Action: We set "Set" and "Reset" at the same time

> The 2 NOR gates are just feeding each other's outputs back as inputs, so the circuit keeps its last state. This feedback loop is what gives the latch its memory

## Flip-flop
- also stores 1-bit information

> Every signal you intend to store in a flip-flop must be driven by exactly **one edge-triggered event**, optionally with a reset condition.

- Flip-flop has an input and a clock input. It responds to input change only at the **clock transistion**. That means, the Flip-Flop is the ***Edge Triggered Memory element***

![[CON - Flip-flop clock signal.png]]

## Combining Computation and Storage
- There are many ways to combine gates for computation and for storage
- Nearly all digital circuits are built as **synchronous circuits** with a global clock signal
	- These circuits don't use single latches as storage, ***but Flip Flops (consisting of 2 latches)*** that are connected to a clock signal

- In a synchronous sequential circuit, the state of the circuit changes only on the rising or falling edge of the clock signal, and all changes in the circuit are synchronized to this clock 
 -> makes the behavior of the circuit predictable and ensures all elements of the circuit change at the same time

![[CON - Sequential Logic Splits Time in Discrete Slices.png]]
> Time is divided in discrete time slices -> called **clock cycles**
> Time between 2 rising clock edges -> called **clock period**

![[CON - happenings between edges and at edge.png]]
> On the rising clock edge the **next (present) value** of every flip-flop becomes the **preset value** that is calculated between edges
### What is the relationship between register and flip-flop?
- A **flip-flop** is a single 1-bit storage element.  
- A **register** is a group of flip-flops that stores n bits of data. These flip flops share the same clock

### Example: Counter
![[CON - Example Counter of Sequential Circuit.png]]
Flow:
- Between clock edges:
	- the register holds the present value (e.g. 3)
	- The logic +1 continuously computes -> the next (preset) value: 4
	- Nothing updates yet - the flip-flops are just holding their value
```
present value = 3
next value = 4
```
 -  At the rising clock edge
	 - The register "feeds" the **next value (4)** from its input
	 - stores that new value internally
	 - Immediately, the **present value** becomes 4
```
present value = 4
next value = 5
```
## Sequential Circuits in SystemVerilog
### `always_ff @(posedge clk or posedge rst)`
- Used to model Flip Flops
- **Important Properties:**
	- Flip flops react only clock or reset.
		- In SystemVerilog: `always_ff @(postedge clk_i or posedge reset_i`
		- In the `always_ff` block, there should be no computations. Otherwise, we would model more than the actual Flip Flop
> We want a strict separation of sequential and combinational logic in SystemVerilog

- The ***sensitivity list*** tells the simulator when to execute block

```SystemVerilog
always_ff @(posedge clk or posedge rst)
```
This means:
- Trigger the block **on every rising edge** of `clk`
- Or whenever `rst` goes **from 0 → 1** (asynchronous reset)

> "Asynchronous" = **not at the same time** / **independent of timing**
> Circuit responds to **asynchronous reset** immediately without waiting for rising clock edge





