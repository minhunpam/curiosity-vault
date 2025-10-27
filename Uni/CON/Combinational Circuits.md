- ***CMOS: Complementary Metal-Oxide-Semiconductor***

## What is Combinational Circuit?
- a digital logic circuit where the output depends solely on the present combination of inputs
- has no feedback or memory component
## Types of Transistors
- **PMOS Transistor**
	- A = 0 -> conducting (GND)
	- A = 1 -> not conducting
![[CON - PMOS Transistor.png]]
- **NMOS Transistor**
	- A = 0 -> not conducting
	- A = 1 -> conducting (supply voltage)
![[CON - NMOS Transistor.png]]

## Logic Gates
![[CON - Logic Gate.png]]
- There are many ways to build logic gates from NMOS and PMOS Transistors
- **CMOS is the most widely used approach** -> fulfill the desired optimization criteria:
	1. minimize the propagation delay (time {input change -> output change})
	2. minimize the power consumption
	3. minimize the area
	4. maximize stability

### What do "Pull-up" or "Pull-down" mean?
- Pull-up == connect to Vdd
- Pull-down == connect to GND

#### "Pull-up and Pull-down  networks are complementary"?
>  Exactly one of them is conducting at any time — never both.

So:

- When **pull-up is ON**, **pull-down is OFF**.
- When **pull-down is ON**, **pull-up is OFF**.

That’s what we mean by **“complementary”** — the two networks behave in **opposite** ways, based on the input.


### CMOS Inverter
![[CMOS Inverter.png]]
![[CMOS Inverter Truth Table.png]]

### CMOS NAND Gate
![[CMOS NAND Gate.png]]
![[CON - NAND Gate Truth Table.png]]

### CMOS NOR Gate
![[CON - NOR Gate.png]]![[CON - NOR Gate Truth Table.png]]

### Multiplexer (MUX)
![[CON - Multiplexer.png]]
- The select signal (sel) determines whether out is equal to $I_0$ or $I_1$
	- Sel = 0 -> out = $I_0$
	- Sel = 1 -> out = $I_1$

### Demultiplexer (DEMUX)
- akes **one input** and **routes it to one of several outputs**, based on the value of **select lines**

## Circuit  Synthesis
- The automated mapping of a circuit to a netlist

### Mapping Boolean Functions to Combinational Circuits
![[Boolean Symbols.png]]![[CON - Logic Functions to Combinational Circuits.png]]

### Mapping a Truth Table to Combinational Circuit
- includes 2 steps:
	1. Truth table -> Boolean Function
	2. Boolean Function -> Combinational Circuit

Example: Implementing an Addition of 3 binary variables
- having 8 possible combinations
- sum each of them up
- re-write the result as a binary number
![[CON - Truth table of adding 3 binary numbers.png]]
- To make the logic function for $s_0$, we only look at lines where $s_0$ gets `true` and build up the logic function using **Disjunction Normal Form (DNF)**
	- Besides we can still use **Conjunction Normal Form (CNF)**  to find the logic function
	> Depends on what is more convenient and optimal

### Describing Combinational Circuits in SystemVerilog
#### (System) Verilog
- powerful and widely used in **Hardware Description Language (HDL)**
	- Besides, there is also **Very High Speed Integrated Circuit Hardware Description Language (VHDL)**

#### Verilog vs. SystemVerilog
- SystemVerilog is an extension of Verilog (all features of Verilog are also available in SystemVerilog)
- Immediate differences
	- Datatypes
		- SystemVerilog: "logic"
		- Verilog: "reg"/ "wire"

### SystemVerilog

#### Application-Specific Integrated Circuit (ASIC)
![[CON - High-level ASIC Design Flow.png]]
1. SystemVerilog
2. Netlist
3. Physical Layout (GDSII)
4. Fab
5. Wafer
6. Packaging
7. Chips on printed circuit board (PCB)

#### Field Programmable Gate Array (FPGA)
- Existing hardware that can be configured to correspond to your circuite ("programmable hardware")

## Writing SystemVerilog
### Styles for Hardware Description
1. **Gate-level**
- The code contains a gate-level description of the circuit
- essentially a textual description of the circuit drawing in the tool Digital

2. Behavioral
- The code contains of a functional description of the circuit
- The level of abstraction is higher, and a synthesis is required to map it to a gate-level netlist

### Overall Setup
- Our hardware is **a box with inputs and outputs**
- We need 2 things:
	1. A method to describe what is "in the box"
	2. A method to test the box

- **Hardware**:
	- **NOT** like classical programming
	- describe the behavior of hardware elements that work in parallel

- **Testbench:**
	- like classical programming
	- generates inputs, checks outputs

![[CON - Hardware as a box.png]]

### Commands
![[CON - commands.png]]


> A Note on the Instantiation of Modules
> 	Instantiation (2 options)
> 		- Named Instantiation (**less error prone**)
> 		- Positional Instantiation
> 	Generate Statements
> 		- allow to create certain parts of a circuit conditionally
> 		- allow to instantiate modules in a loop (one instance for each wire of a bus)

### Operators
![[CON - Operators in SystemVerilog.png]]

#### Left Shift and Right Shift Operators #Bitwise
- are binary bitwise operators that are used to shift the bits either left or right of the first operand by the number of positions specified by the second operand allowing efficient data manipulation

##### Left Shift (<<) Operators
- The left shift(<<) is a binary operator that takes two numbers, left shifts the bits of the first operand, and the second operand decides the number of places to shift
$$
	(a << b) = (a\cdot2^b)
$$

###### Example:
```
21: 00010101
42 = 21 << 1 : 00101010 <- #(zeros added to the end) = b
```

##### Right Shift (>>) Operators
- a binary operator that takes two numbers, right shifts the bits of the first operand, and the second operand decides the number of places to shift

$$
(a >> b) = (\frac{a}{2^b})
$$
###### Example:
```
21: 00010101
42 = 21 >> 2: 00000101 <- #(zeros added to the end) = b #(bits discarded from the number when out of bit range)
```

##### Important points of Shift Operators
1. Shift operators should not be used for negative numbers
2. If the number is shifted more than the size of the integer, the behavior is undefined

#### Bitwise And ($\land$) and Bitwise Or ($\lor$) #Bitwise 
##### Bitwise And ($\land$)
- takes 2 numbers as operands and does AND on every bit of 2 numbers
- The result of AND is 1 iff both bits are 1
```C
unsigned int a = 5; // 00000101 - 8-bit binary
unsigned int b = 9; // 00001001 - 8-bit binary

// a & b:
// a = 00000101
// b = 00001001
// >>  00000001
```

###### Application: Determine if a number is odd or even
- We can apply the AND operator between the to-determine number with 1, which indirectly takes the least significant bit

##### Bitwise Or ($\lor$)
- takes 2 numbers as operands and does OR on every bit of 2 numbers
- The result of OR is 1 if any of the 2 bits is 1
```C
unsigned int a = 5; // 00000101 - 8-bit binary
unsigned int b = 9; // 00001001 - 8-bit binary

// a | b:
// a = 00000101
// b = 00001001
// >>  00001101
```

### The Testbench
```SystemVerilog
ifndef SYSTHESIS
module testbench();

// Here we define that input and output wires
	// registers for inputs to the module
	logic in_a;
	logic in_b;
	logic in_c;
	
	// wires for the outputs of the module
	logic out_q;
	
// Here we instantiate DUT
	
	// create a simple_circuit and connect the logic elements
	simple_circuit circ(
		.in_a(in_a),
		.in_b(in_b),
		.in_c(in_c),
		.out_q(out_q)
	);

	
	// create dump file
	initial begin
		$dumpfile("simple_circuit.vcd");
		$dumpvars(0, testbench);
	end

// We put the testbench into an "initial" block:
// NOT SYNTHESIZABLE
// Execution starts with the start of the simulation

	// this happens only, you can use this to test your circuit
	initial begin
		in_a <= 0;
		in_b <= 0;
		in_c <= 0;
		#1 
		
// Display Statment - NOT SYNTHESIZABLE		
		$display("[Time: %04t, TESTCASE 1] in_a %d, in_b = %d, in_c = %d results in output = %d ", $time, in_a, in_b, in_c, out_q);
// Time Delay - Not Synthesiable
		#1 
		
		in_a <= 1;
		in_b <= 0;
		in_c <= 0;
		#1 
		$display("[Time: %04t, TESTCASE 2] in_a %d, in_b = %d, in_c = %d results in output = %d ", $time, in_a, in_b, in_c, out_q);
		#1 
		
		
```

#### What are Synthesizeable and Non-synthesizable Code?
- **Synthesis Tool**: translates Verilog/ VHDL code into gates, registers, RAMs, etc. (something that FPGA can understand)
- Synthesizable code is something that FPGA can understand
- In contrast, there is also non-synthesizable code
	- The reason of non-synthesiable code is it makes your testbenches more powerful


### Numbers #CON_Important
![[Number in SystemVerilog.png]]

### Blocking vs. Non-block Statements
- **Blocking Assignments (=)** -> done immediately
	- The assigned value propagates immediately to all signals that depend on the value

```systemverilog
module blocking_assignment {
	input logic a,
	input logic b,
	input logic c
};
	
	initial begin
		a = 1;
		b = a; // b gets 1 (because a has been already updated)
		a = 0;
		c = a;
	end
endmodule
```

> Each line **waits for the previous line to complete**

- **Non-Blocking Assignments (<=)** are not done immediately but are **scheduled to happen when time advances** (i.e. when the simulator advances from the current time slice to the next)
	- Non-block assignments do not impact operations that happen within the same time slice

```SystemVerilog
module non_blocking_assignment {
	input logic a, // a = 0
	input logic b, // b = 0
	input logic c // c = 0
} 

	initial begin
		a <= 1; 
			// scheduled a -> 1 after the block finishes
		b <= a;
			// scheduled b -> a's old value == 0
		a <= 0;
			// schedueled a -> 0 (overwrite  the previous schedule)
		c <= a;
			// scheduled c -> 0
	end
 
 // after the block finishes
 // a -> 0
 // b -> 0
 // c -> 0
 
endmodule
```

#### `assign` keyword
- used to create **continuous assignment**. That means: the expression on the RHS is continuously driven onto the signal on the LHS

> ***A signal that is continuously driven by an `assign` cannot be reassigned elsewhere***

#### When to use `assign`
- **Outside procedural blocks** (`always_ff`, `always_comb`, or `initial`)
- To drive **nets** (typically `wire` or implicitly declared wires)
- For **combinational logic that’s always active**

#### When not to use `assign`
Inside **procedural blocks**, such as:
- `always_ff` (for sequential logic)
- `always_comb` (for combinational logic inside a block)
- `initial` (for simulation-only initialization)
Instead, assign using:
1. `=` ->  blocking assignment
2. `<=` -> non-blocking assignment
### `always_comb`
- is executed upon every update of every signal that is used as input in this block
- The inputs that are used in the block are called **sensitivity list**

#### Important properties:
- can assign a value to logic signals multiple times (the last one counts)
- does not matter if you use a lot "code" to describe your circuit (**does not imply lower performance or larger area, just means it is a BETTER DESCRIPTION)**
- a functional description of how to map the inputs to the outputs ("this function is executed whenever there is an update of an input")

### Signal "Responsibility"
- It is possible to have multiple assign statements and multiple `always_comb` blocks in a module

#### Strict Separation of Responsibility:
1. Each signal can only be driven by ***exactly 1 assign statement or 1 `always_comb block`***
2. It is not possible that a signal is sometimes handled by 1 `always_comb` block and sometimes by another
3. For each combination of inputs, each code block always need to define the values of ALL signals it is responsible for

