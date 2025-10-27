## Design
- Initially, after the reset (_meaning `rst_i == 1` _), 
	- `busy_o <= 0` 
	- `valid_o <= 0` 
- Only when `busy_0` = 1, FSM reacts to `start_i`
- When `start_i == 1` , the computation starts and leaves `INIT` to come to `COMP_D`
	- `busy_o <= 1` 
- When leaving initial state, `a_i` is stored to **an internal register** (in this case: `x`) -> ensures that changing `a_i` during computation does not affect the outcome
- When computation is finished:
	- `valid_o <= 1`
	- the computed result is applied to the output: `result_o <= r`
	- `busy_o <= 0`
- Afterwards, the FSM returns to the initial state and waits until a new computation is started.

## Implementation of Finite State Machine
![[CON - Task 1 - FSM of Integer Square Root.png]]
- Create registers to hold the current state and next state
- The implementation of the Finite State Machine is done with a combinational circuit. Because I only want to get the next state

## Implementation of the Algorithm
![[CON - Task 1 - SquareRootAlgorithm.png]]
- Because this is an iterative algorithm, which means the previous result of a signal shall be needed for the next calculation. Therefore the implementation of this algorithm will be held in a sequential circuit (`always_ff @(posedge clk_i or posedge rst_i))
- This `always_ff` block will is only triggered in 2 cases:
	1. When `rst_i` rises - Set all triggers to reset values
	2. When `clk_i` rises - State update
> As far as I understand and imagine, the reset signal (`rst_i`) will only rise once to set everything to a starting condition, then stay low unless I want to restart the machine. The `always_ff` block will accordingly be triggered by solely the rising edge of the clock

## Bugs that I encountered
### 1. ERROR: Multiple edge sensitive events found for this signal!
My error code
```SystemVerilog
always_ff @(posedge clk_i or posedge rst_i) begin
	if (rst_i) begin
		// ...
		valid_o <= 0; // updating `valid_o` on reset
	end else begin
	end
	
	case (state_p) // updates `valid_o` and other outputs
		INIT: begin
			// ...
			valid_o <= 0; // updating `valid_o` on reset
		end
		
		COMP_D: begin
			// ---
			valid_o <= 0;
		end
		
		SQRT: begin
		end
		
		default: ;
	endcase
end
```
- With the structure from the code snippet above, `valid_o` receives 2 assignments:
	1. From the asynchronous reset branch (when `rst_i` is high)
	2. From the logic that is meant to run on the clock edge
	This can be interpreted  as 2 independent edge controls acting on the same flip-flop

#### Solution:
- Including the case inside the `else` branch

### 2. Use `default` in a wrong way -> leads to Timeout when `make test`
```SystemVerilog
always_comb begin
	state_n = state_p;
	case (state_p)
		INIT: begin
			if (start_i != 0) begin
				state_n = COMP_D;
			end
		end
	
		COMP_D: begin
			if (d <= x) begin
				state_n = SQRT;
			end
		end
		
		SQRT: begin
			if (d == 0) begin
				state_n = INIT;
			end
		end
		
		default: state_n = INIT; // should never happen

	endcase

end
```
- The `default` branch will never run if one of the states matches. But when the condition inside a state is `false`, there is now no assignment to `state_n`, so in `always_comb` block, `state_n -> X` (every signal assigned in combinational circuit must have a value)
- On the next clock edge, `state_p <= `state_n (X) -> FSM fails -> testbench times out

#### Solution: 
- Give `state_n` a deterministic default before the `case`
- Add else branch in each case, but this is redundant!

## Roles of `start_i`,`busy_o` and `valid_o`?
- are control signals - don't perform computation themselves but coordinate when computation begins, in progress, and done

