## Combinational Circuits #CON #Block1 
- Complementary Metal-Oxide-Semiconductor (CMOS)
	- uses PMOS and NMOS transistors
		- `pMOS` (+) transistor:
			- A = 0 -> conducting (connected to `GND` (ground))
			- A = 1 -> NOT conducting
		- `nMOS` (-) transistor: 
			- A  = 0 -> NOT conducting
			- A = 1 -> conducting (connected to `Vdd` (positive supply voltage))
	- is the technology of almost all digital circuits (from contactless RFID chips to server CPUs)

	- Need 2 things from transistors
		- Computation (Combinational Logic) 
			- how to apply a function to input data to generate an output?
		- Storage (Sequential Logic)
			- How to store data and intermediate results of computations?

### Computation - Logic Gates
- There are many ways to build Logic Gates from NMOS and PMOS transistors, but **CMOS** is the most widely used approach, because it fulfills the desired **Optimization Criteria** for logic gates best

- Important **Optimization Criteria**:
	- minimize the propagation delay (the time it takes until an input change leads to a change at the output)
	- minimize power consumption
	- minimize the area (area corresponds to the cost in production)
	- maximize stability (noise should not change values)
- 
	
	