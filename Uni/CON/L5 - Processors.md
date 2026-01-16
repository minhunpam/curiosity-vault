## Motivation
- We want to build a general purpose hardware that is configured via an input (instructions) for a particular application

## Von Neumann Model  & Havard Architecture
![[CON - Von Neumann Model.png]]
- Von Neumann Architecture: Memory is kept as a whole
- **Havard Architecture: Memory is splitted into**
	- **Data Memory**
	- **Instruction Memory**
### 1. Processing Unit
- Constitutes the data path of the CPU
- Based on control signals that are provided as input operations performed in the ALU and data registers are updated
![[CON - Processing Unit.png]]
#### 1. Arithmetic Logic Unit (ALU)
- a combinational circuit performing calculation operations
- `alu_sel` is the input to the Multiplexer
#### Basic properties
- Takes 2 n-bit inputs
- Performs an operation based on 1/both inputs
	- The performed operation is selected by the control input `alu_sel`
- Returns an n-bit output
	- Typically also provides a status output with flags
		- Example:
			- To indicate overflows or relations of A and B (`A == B`, `A < B`)
![[CON-ALU.png]]
#### 2. Register File
- ***Is a array of $m$ $\text{n-bit}$ processor registers***
- In a given clock cycle 1 n-bit value can be stored in the register selected via the **address signal** $R_w$ 
	- In case **control signal** $RegWrite$ is 0, no register is written
> $R_W$: chooses which register index to write
> $RegWrite$: determines if writing actually happens (high/low)

##### WRITEBACK to Register File
- Each instruction that produces a result will **write that result** into one of the values of registers

- In each cycle, 2 registers can be read and are provided at the outputs `Read Port A` and `Read Port B`
	- The registers to be read are selected via $R_A$ and $R_B$
$\Rightarrow$ ***The register file is essentially a memory with 1 write port and 2 read ports***

##### Data Registers
- In case of RISC-V, the register file consists of 32 registers
	- 5 bits are needed for $R_W, R_A, R_B$
- Data registers with 2 output MUX
- Input is stored in any one of the registers on the next rising clock edge (selected via $R_w$ signal)
- Typical register size: 8, 16, 32, 64 bit
![[CON - Data Registers.png]]
### Control Unit
#### 3. Instruction register
- Stores the instruction that shall be executed by the data path
- **The instruction decoder** 
	- Figures out what the instruction means
	- Produces all the control signal needed to execute that instruction
![[CON - Instruction Decoding.png]]
#### Instruction Set Architectures (ISA)
- An instruction is the basic unit of processing on a computer
	- Set of all instructions on a given computer architecture
- The ISA is the interface between hardware and software

##### Options to represent instructions:
1. Machine language:
	- A sequence of 0s and 1s (`0x83200002`) -> This is the sequence of zeros and ones the processor takes into its **instruction register** for decoding and execution
2. Assembly language:
	- This is human-readable representation of an instruction
		- `ADD x3, x1, x2`

- There are many Instruction Set Architectures from different vendors
	- Examples: `X86`, `AMD64`, `ARM`, `MIPS`, `PowerPC`, `SPARC`, `AVR`, `RISC-V`
- Instruction sets vary significantly in terms of number of instructions
	1. ***Complex Instruction Set Computer (CISC)***
		- Many kinds of instructions can directly read from OR write to memory, not just dedicated load and store instructions
```assembly
ADD AX, [BX]    ;Add value from memory (at address in BX) directly to register AX
```
- Here:
	- The `ADD` instruction **accesses memory (`[BX]`)** and **performs arithmetic** - all in 1 instruction
	- Design philosophy: many instructions, few instructions also for complex operations
	- Hundreds of instructions that include instructions performing complex operations like entire encryptions
	
	2. ***Reduced Instruction Set Computer (RISC)***
		- Load/store architectures: only dedicated load and store instructions read/ write from/to memory
		- Fewer instructions, lower complexity, high execution speed
		- Instruction sets including just basic operations
		
	3. ***One Instruction Set Computer (OISC)***
		- Computers with a single instruction (academic), e.g

## [[RISC-V Basics]]

## Memory
- Memories are RAMs
	- Its randomness is represented in a sense that it can access any memory location
	- Reading from memory
	- Writing to memory
### Memory word 
- a natural data size of the processor (the number of bits the CPU can process at once)
	- one word = 4 Bytes (32-bit system) -> takes up 4 addresses
	- one word = 8 Bytes (64-bit system) -> take up 8 addresses
#### Word alignement
- Many CPUs require that words start at variable that are multiples of the word size:
	- 4-Byte word -> address must be divisible by 4 
	- 8-Byte word -> address must be divisible by 8
### Address
- Identifies a single byte
- In almost all modern architectures, **memory is byte-addressable**, meaning:
	- Address `0x1000` refers to 1 byte
	- Address `0x1001` refers to the next byte
	- Address `0x1002` referes to next byte 
### Endianess
#### What is Endianness?
- Refers to the order in which bytes are arranged in memory
- If one computer reads bytes from left -> right and another reads them from right to left, **issues arise when these computers need to communicate**
- Endianness ensures that bytes in computer memory are read in a specific order
- Endianness comes in two primary forms: Big-endian (BE) and Little-endian (LE).
	- **Big-endian (BE)**: 
		- Stores the MSB (the **"big end"**) first. 
		- This means that the first byte (at the lowest memory address) is the largest, which makes the most sense to people who read **LEFT TO RIGHT**.
	- **Little-endian (LE)**: 
		- Stores the LSB (the **"little end"**) first. 
		- This means that the first byte (at the lowest memory address) is the smallest, which makes the most sense to people who read **RIGHT TO LEFT**.
#### Big-endian
For instance, a 32-bit integer `0x12345678` would be stored in memory as follows in a big-endian system:
```
Address:   00   01   02   03
Data:      12   34   56   78
```

#### Little-endian
For the same 32-bit integer `0x12345678`, a little-endian system would store it as:
```
Address:   00   01   02   03
Data:      78   56   34   12
```


### Basic idea of memory design
- Memories are built using so-called memory cells. Each cell can store one bit
- The memory cells are placed on a chip next to each other and form a rectangular structure: ***cell array***
![[CON - Memory Design.png]]
- A bitline connects all memory cells of a column vertically (yellow)
- A worldline connects all memory cells of a row horizontally

- This basic structure is used for all kinds of memories:
	- Non-volatile memory (Flash Memory)
	- Static Memory
	- Dynamic Memory (DRAM)
	- DDR Memory

#### Basic Idea of Read/Write for DRAM
- A DRAM cell just consists of a single transistor and a capacitance that stores the data value
- In steady state (no access) all bitlines and wordlines are disconnected from the power supply (i.e. they are floating)

##### Writing a cell:
- Set a corresponding bitline to the desired storage value
- Set corresponding wordline to high
--> This charges the capacitance of the desired cell to the desired storage value

##### Reading a cell:
- Pre-charge the corresponding bitline to the desired voltage value
- Disconnect the bitline
- Set the corresponding wordline to high
$\rightarrow$ The bitline keeps its value, if the stored value is high or is pulled to low, if the stored value is zero

##### Read/ Write access is done for entire row:
- All bitlines are set to the desired storage or pre-charge value
- One wordline is set to high

### Basic Idea of Load/Store Instructions
- Load instructions copy data from the data memory to the register file
- Store instructions copy data from the register file to the data memory
- The register file acts like a "workbench"
- The data memory acts like a more long-term "storage space"

## Instruction Memory
- The instruction memory stores a sequence of instruction
- The program counter is incremented by 4 in each and reads one instruction after the other
	- **The program counter is a special register in the CPU that always contains the address of the next instruction to fetch**
- This allows executing a static batch of instructions
	- Instruction memory holds a fixed sequence of instructions
	- The PC simply moves forward through them sequentially
![[CON - INSTRUCTION MEMORY.png]]
Step-by-step:
- Process begins: 
	- **Program Counter** sends an address to **Instruction Memory**
	- **Instruction Memory** returns the machine code instruction stored at that address
	- The CPU decodes and executes that instruction
	- At the same time, the **Program Counter**
		- Increment the instruction address by 4 - Normal Instruction
		- PC + offset - Branch
		- new PC = jump target
		- CPU stall: PC = PC (for multi-cycle instruction) - until is done
		- CPU halt: PC = PC forever
- The process repeats




