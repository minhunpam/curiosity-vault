## Motivation
- We want to build a general purpose hardware that is configured via an input (instructions) for a particular application

## Von Neumann Model  & Havard Architecture
![[CON - Von Neumann Model.png]]
### 1. Arithmetic Logic Unit (ALU)
- a combinational circuit performing calculation operations
- `alu_sel` is the input to the Multiplexer
#### Basic properties

### 2. Register File
- contains m n-bit registers
- The register file is esesntiall a memory with 1 write port and 2 read ports

#### Data Registers
- In case of RISC-V, the register file consists of 32 registers

##### Basic registers
- Data registers with 2 output MUX
- Input is stored in any one of the registers on the next rising clock edge (selected via $R_w$ signal)
- Typical register size:
	- 8
	- 16
	- 32 
	- 64

### 3. Instruction register
- Stores the instruction that shall be executed by the data path
- The instruction decoder maps the instruction to control signals

## Instruction Set Architectures (ISA)
- An instruction is the basic unit of processing on a computer
- The ISA is the interface between hardware and software
- There are many ISA from different vendors

### Options to represent instructions:
1. Machine language:
	- A sequence of 0s and 1s 

### Instruction sets vary significantl in terms of number of instructions
1. Complex Instruction Set Computer (CISC)
2. Reduced Instruction Set Computer (RISC)
3. One Instruction Set Computer (OISC)

## Memory
- Memories are RAMs
	- Its randomness is represented in a sense that it can access any memory location
	- Reading from memory
	- Writing to memory
- Memory word (= 4 Bytes = 32 bit)
-
### Endianess
- 2 options for the sequence of storing the bytes of a word in memory:
	1. Lilttle endian: LSB (least significant byte) - at the lowest address
	2. Big endian: MSB (most significant byte) - at the lowest address
![[CON - endianess (little and big).png]]

### Basic idea of memory design
- A bitline connects all memory cells of a column vertically (yellow)
- A worldline connects all memory cells of a row 