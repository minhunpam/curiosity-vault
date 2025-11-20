1. Implement an 8-bit divider as a RTL model in SystemVerilog
2. Integrate this divider to the MircroRISC-V CPU by adding a division instruction according to RISC-V instruction set

## Divider
### Components
- `clock_i` , `reset_i` - input signals for every synchronous machine
- `dividend_i`, `divisor_i`, `quotient_o` - division terms
- `start_i`,`busy_o`, `finish_o` - status signals indicating the progress of the computation

### Algorithm: Division by subtraction algorithm
- In each clock cycle
	- subtract `divisor` from the `dividend` per clock cycle until the `dividend < divisor`
	- Each iteration increments a temporary variable - counter of the number of subtractions which gives us the quotient
		- must satisfy $0 \leq dividend - quotient \times divisor < divisor$ 
### Registers
1. `state` - current state of the finite state machine
	 - `INIT`
	 - `BUSY`
	 - `FINISH`
2. `quotient` - the counter 
	- Increments with each subtraction and thus gives the final quotient
3. `subtrahend` (= quotient * divisor) 
	- Initialized with the dividend
	- Subtractions are applied together with `divisor_i`

> No need to create additional registers for divisor and dividend; instead, use directly `divisor_i` and `dividend_i`
### Protocol:
1. `if (start_i && clk_i)`
	- A -> value of signal `dividend_i`
	- B -> value of signal `divisor_i`
2. After a finite amount of clock cycles, let `finish_o` become high
3. If `finish_o` is high
	- If B was 0, `quotient_o` must have value -1
	- Otherwise, `quotient` must have value $\lfloor \frac{A}{B} \rfloor$ 
## MicroRISC-V Integration
- Task: extend the CPU with the DIVU instruction of RISC-V's M extension
- `DIVU rd rs1, rs2` 
	- Performs a 8-bit/8-bit division between `rs1` and `rs2` and store the `8-bit` result in register `rd`
	- If the dividend is zero, the result in register `rd` should be -1 (all bits set)
> ***Do not forget to sign-extend the division result to 32-bits!***
> ***In RISC-V, the DIVU instruction performs a 32-bit unsigned division***

- Instantiate the divider within the CPU
- Extend the decoding and executing steps of the CPU for the divider
> ***The division operation is a multi-cycle operation. Thus, it requires to stall the CPU until the divsion algorithm finishes and its result is available***

### Overview:
- The decoder will detect the instruction `DIVU` based on `opcode`, `funct7`, `funct3`. If that is the case, then set control signals correspondingly
	- `alu_reg_write`, because `DIVU` is a R-Type Instruction writing to `rd`, but due to many cycles, we don't want to write immediately, but delay that
- Instantiate the divider module with correct signals
	- The `divu_start` is continuously driven by signals `is_divu`, `divu_busy`, `divu_finish
- The divider only starts once, when it hasn't done any computations and hasn't finished yet
- Stall the CPU while the divider is busy
- 

## What I struggled to understand
### 1. What are `busy_o` and `finish_o`?
- **Status outputs** of the FSM -> **tell the outside world (CPU, testbench)** what the divider is doing
	- do not control the divider 
		- This kinda signal is used to affect the FSM transition, but it can be used to decide ***when to update an output register***
	- do not change the algorithm
- `busy_o` -> report the divider is still computing
- `finish_o` -> signal the output is ready now
- These kinda outputs are combinational -> only allowed in `always_comb`
	- In order to want to use kinda `finish_o` inside `always_ff`, we have to declared a registered control signal
- Compute correct result for DIVU
	- Extend the division result to 32-bit
- Insert divider result into writeback


#### Different from `busy_o` and `finish_o`, inputs like `start_i`, `clk_i` and `reset_i` actually do controll the internal flow

### 2. Does the sequential logic run over and over?
- The sequential logic runs on **EVERY CLOCK EDGE** forever
	- reacts to:
		- clock ticks
		- power is on
- The combinational logic runs whenever inputs change
	- reacts to:
		- state
		- input signals

### 3. "Do not use output signals as internal control inputs"
- In previous implementation, instead of having 1 if branch for the `reset_i` and 1 else for `clk_i` branch in `always_ff` block, I just spawned a different branches in the scope as them
	- This accidentally mixes with state transitions, which created ambiguity
	```SystemVerilog
	if (reset_i) 
	...
	else if (finish_o) // <--- wrong
	else
	```
- Instead, we have to put the `else if(finish_o)` inside `else` branch. This will guarantee updates inside `if(finish_o)` always executes on every positive clock edge

### 4. What does it mean to "stall CPU"?
- Our context: multi-cycle operations work inside a single-cycle CPU like MircoRisc-V
- ***Stalling the CPU = freezing the CPU so it temporarily stops executing new instructions***
		This means:
	- PC doesn't change
	- Register File doesn't write anything
	- CPU doesn't fetcht the next instruction
	- ***The CPU stays in the same instruction for several cycles***

### 5. What does "instantiating a module" mean?
- Place the divider hardware block inside the CPU datapath and connect it to the correct CPU signals

## Bugs that I encountered
### 1. Driving 1 signal from Both `always_ff` and `always_comb` -> Race condition
- The solution is the drive it in either `always_ff` or `always_comb`
### 2. Assigning outputs inside `always_comb`
- The solution for this to put in the `always_ff` block
```SystemVerilog
if (finish_o) begin
	quotient_o <= quotient_p;	
end
```
- How this actually works:
	 - Cycle 1 (BUSY State)
	```SystemVerilog
	finish_o = 1 // combinational conditio in BUSY State
	state_n = FINISH
	```
	- Clock Edge between Cycle 1 -> Cycle 2
		 1. FSM `state_p` becomes `FINISH`
		2. `always_ff` evaluates
		```SystemVerilog
		if (finish_o) begin
			quotient_o <= quotient_p;
		end
		```
		- `finish_o` is 1 at the clock edge because it was previously in during Cycle 1
		- The non-blocking assignment:
			- Phase 1 - clock edge triggers the evaluation
				- "Update `quotient_o` with `quotient_p` later"
				- The assignment doesn't take effect yet
			- Phase 2 - Time advances
				- The assignment to `quotient_o` is applied
	- 
### 3. `FINISH` State lasts zero
- Timeline:
	- BUSY - Cycle 1 
		- `finish_o = 1`
		- `state_n = FINISH`
	- Clock edge between cycle 1 -> cycle 2
		- `state_p = FINISH
		- `quotient_o` is expected to get value from `quotient_p`
	- But when it comes to `FINISH` case, `finish_o` turns to 0, which makes the update of `quotient_o` fails, because `finish_o` is supposed to act like a **valid output handshake** to give outside modules ***ONE FULL CLOCK CYCLE*** to detect the pulse

