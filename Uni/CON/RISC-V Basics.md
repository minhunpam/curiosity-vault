## RISC-V Instruction Sets
- Base Instruction Sets
	- RV32I
		- only allows 16 registers
### RV32I
- The ALU and the register file are all 32 bit
- Our register file consists of 32 registers
	- ***Note: register x0 always reads zero; writing to x0 does not lead to storing a value***

### Basics
- The base instruction set has fixed-length
#### R-Type Instruction
- These are instructions that perform arithmetic and logic operations based on 2 input registers
![[CON - R type Instruction.png]]
- `funct7`, `funct3`, `opcode` define the operation to be performed
- `rs1` defines source register 1
- `rs2` defines source register 2
- `rd` defines the destination register

- The image above describes addition instruction in machine language, where:
	- `funct7`, `funct4`, `opcode` represents the instruction

- This instuction can be represented in human readable with ***Assembly***
```assembly
ADD, R3, R2, R1
```

- The RV32I Instruction Set contains 40 instructions with different catergories:
	- Integer Computational Instructions
	- Load and Store Instructions
	- Control Transfer Instructions
	- Memory Ordering Instructions
	- Environment call and Breakpoints

### Integer Computational Instructions
![[CON - Integer Computational Instructions.png]]
- All instructions take 2 input registers (`rs1` and `rs2`) and compute the result in `rd`
- Example: `sub r3, r1, r2` -> computes: r3 = r1 - r2

#### Subcategories
- ***Logic Functions***
	- `AND`
	- `OR`
	- `XOR`
- ***Arithmetic***
	- `ADD` (Addition)
	- `SUB` (Subtraction)
- ***Shifts***
	- `SLL` (Logical Shift Left)
	- `SRL` (Logical Shift Right)
	- `SRA` (Arithmetic Shift Right)
- ***Compares***
	- `SLT` (Set on Less Than)
	- `SLTU` (Set on Less Than - unsigned)

## Load Word (right -> left)
- Assembly: 
```assembly
LW rd, offset(rs1)
```
- **Load a 32-bit word from memory at address `rs1 + offset` and store it into register `rd`**

![[CON - Load word.png]]
- `imm[11:0]` - 12-bit immediate offset
	- Is added to the base register (`rs1`) to computer the effective memory address
	- Example: `LW x5, 8(x10)`
		- Actual memory: `x10 + 8 Bytes`

- Functionality:
	- Loads a word (32 bits/ 4 Bytes) from memory into a register
	- Example applications
		- Load data from a pointer by setting offset to zero (`LW rd, 0(rs1)`)
		- Load data from a pointer by providing a relative offset (`LW rd, offset(rs1)`)
		- Load data from a fixed address at offset 0 by setting to `rs1` to `x0` (`LW rd, addr(x0)`)

### Why do we need an offset?
- Because the base pointers often point to structure, arrays, stack frames
- We rarely want to always load the first word at a pointer, but load data inside the object
```C
struct Foo {
	int a; //offset 0
	int b; //offset 4
	int c; //offset 8
};
```

```assembly
LW x5, 0(x10) 
LW x6, 4(x10)
LW x7, 8(x10)
```
- All 3 loads use the same base pointer `x10`, but they load different words because of the offset
### More Load Instructions
![[CON - Load Instructions.png]]
- `LBU` - Load Byte Unsigned - like `LW` - but load 8 bit
- `LHU` - Load Halfword Unsigned - like `LW` - but load 16 bit
- `LB` - like `LBU` - perform sign extension for upper bits
- `LH` - like `LHU` - perform sign extension for upper bits

## Aligned and Non-aligned Data Accesses
- **Aligned Access:**
	- Read operation from location 0, 4, 8, C,
	- The word to be read corresponds to a word memory location that can be read with a single read operation

- **Non-aligned Access:**
	- Reading a word from any other address is a misaligned memory access
	- The word to be read requires to read 2 words from memory
		- The parts of the 2 words need to be combined to deliver the word that is requested by the load
		- Example: load from address 03 (marked in green)
![[CON - non-aligned access.png]]

## Store Word (left -> right)
- Assembly:
```assembly
SW rs2, offset(rs1)
```
- Store 32-bit word from `rs2` into memory at that address (`rs1 + offset`)
- Unlike load instruction, store instructions don't have `rd` anymore

![[CON - store word.png]]
- `imm[11:5]` - 7 bits - high 7 bits of the offset
- `imm[4:0]` - 5 bits - low 5 bits of the offset

- Functionality:
	- Store a word (32 bits/ 4 Bytes) to memory
	- Example applications:
		- Store data to a pointer in a register by setting offset to 0 (`SW rs2, 0(rs1))
		- Store data to a pointer + offset (`SW rs2, offset(rs1)`)
		- Store data to an absolute address (`SW rs2, addr(x0`))
### Why is 12 bit splitted into 7 high bits and 5 low bits
- Different from I-Type which uses bits `[11...7]` for `rd`, we can't reuse that as-is, because there is no `rd` 
- We want to reuse as much of the decode hardware as possible
	- `rs1`, `funct3`, `opcode` doesn't change
```
| 31........20 | 19..15 | 14..12 | 11..7 | 6.....0 |
|     ????     |   rs1  | funct3 |  ???? | opcode  |
```
- The top 12 bits and the bottom 5 bits are also free, but we can't just insert `rs2` into the bottom 5 bits, so the logical place for `rs2` is right next to `rs1`, since both of them are are **source registers**. 
- Once the `rs2` is placed, the immediate 12-bit would be splitted correspondingly

### More Store Instructions
- `SB` - Store Byte - same `SW` - store the lowest 8 bit of `rs2`
- `SH` - Store Halfword - same `SW` - store the lowest 16 bit of `rs2`

## `ADDI` (Add Immediate) (right -> left)
- Assembly: 
```assembly
ADDI rd, rs1, immediate
```
- computes: `rd = rs1 + immediate`
![[CON - ADDI MACHINE LANGUAGE.png]]
- Functionality:
	- Computes: `rd = rs1 + imm`
	- Example applications
		- Move content of one register to another register by setting immediate to 0 (`ADDI rd, rs1, 0)`
		- Set a register to a constant value by using `x0` as source: `ADDI rd, x0, immediate`
		- Increment/ Decrement a register by setting `rd = rs + 1` : `ADDI x1, x1, 1`

## What is an immediate?
- An immediate is a constant value encoded directly inside an instruction
	- It is not stored in memory
	- It is not loaded from a register
	- It lives inside the machine code itself
#### What is the point of immediates?
1. You don't want to waste a register just to store small constants
2. Much faster than loading constants from memory
3. Critical for stack pointer and memory addressing
	```asm
	addi sp, sp, -16    # allocate 16 bytes on stack
	addi sp, sp, 16     # deallocate
	```

### Difference from `ADD`
- `ADDI` is to add immediate
- `ADD` is to add registers
	- Both operands come from registers
	- No immediate allowed
	- Used when the value is not constant


### More operations with Immediates
- `LUI` - allows to load 20 bits into upper bits of a register
	- together with `ADDI` this allows to set a register to a 32 bit constant value
- `SLTI` - sets the register `rd` to 1 if `rs1` is less than the sign-extended immediate
- `SLTIU` - is the unsigned version

- `XORI`, `ORI`, `ANDI` are logic operations with immediates
- `SLLI`, `SRLI`, `SRAI` are shift operations, where the 5 bit immediate "shamt" defines the shift amount

## BEQ (Branch if EQual)
- Assembly:
```assembly 
BEQ rs1, rs2, offset
```

```
if (rs1 >= rs2) branch to offset (pc = pc + offset)
else            continue with next instruction (pc = pc + 4)
```


> ***This is how loops, if-statement, and branches work in RISC-V***

![[CON - BEQ.png]]
- The immediate is split in 4 pieces
	- `imm[12]` -> bit 31
	- `imm[10:5]` -> bit 30 -> 25
	- `imm[4:1]` -> bit 11 -> 8
	- `imm[11]` -> bit 7

- Functionality:
	- Branch if equal to address PC + `imm * 2`
	- RISC-V branch offsets are always even numbers. This is because all RISC-V instructions are 4-byte aligned, so the lowest bit of any instruction address is always 0. That means:
		- No branch ever jumps to an odd address -> No need to store that 0 in the immediate
	- So RISC-V saves 1 bit by NOT storing the lowest bit of the offset. Instead, the CPU automatically adds it back by doing bit-shifting to the left by 1

## More Conditional Branches
![[CON - more conditional branches.png]]
- `BNE` - Branch if not equal
- `0BLT` - Branch if less than
- `BGE` - Branch if greater or equal
- `BLTU` - Branch if less than unsigned
- `BGEU` - Branh if greater or equal

## JAL/JALR
- `JAL` - Jump and Link
	- Performs an unconditional jump to PC + imm*2
	- Saves the return address (the address of the next instruction) into `rd`
	- It jumps to the target label (PC-relative jump)
	- Example applications:
		- Unconditonal jump (`rd` is set to `x0` in this case)
		- Subroutine call (will be discussed later)
![[JAL.png]]
- `JALR` - Jump and Link Register
	- Save the return address (the address of the next instruction) into `rd` (`rd = PC + 4`)
	- Jump to a target (`target = rs1 + offset`)
	- Jump to target (with LSB = 0) - `PC = target & ~1`
		- Because RISC-V instructions are always 4 Bytes and must be aligned. To avoid jumping into the middle of an instruction, `jalr` resets LSB to 0
	- Update PC
	- Example: 
		- `0x1000:    jalr x1, 20(x2)`
		- Registers:
			- `x2 = 0x2000`
			- `PC = 0x1000`
		- Computer the return address: `x1 = PC + 4 = 0x1004`
		- Compute the jump target: `target = x2 + 20 = 0x2000 + 0x14 = 0x2014`
		- Force LSB to 0 (alignment): `target = 0x2014 & ~1 = 0x2014`
		- Update: `PC = 0x2014`
	- Example applications
		- Subroutine call/return (well be discussed later)
		
	- Special case: `jalr zero, 0(ra)`
		- If **rd = x0(zero register), the return address is not saved**


## Arithmetic Instructions
- `ADD rd, rs1, rs2` --> `rd = rs1 + rs2`
- `SUB rd, rs1, rs2` --> `rd = rs1 - rs2`

## Writing a First Program
- Load values from memory address `0x20`, `0x24` into registers
- Add the registers together
- Store the result back to memory at `0x28`
- Halt the CPU

### A first mapping
```
LW rd=x1 rs1=x0 offset=0x20
LW rd=x2 rs1=x0 offset=0x24
LW rd=x3 rs1=x1 rs2=x2
SW rs2=x3 rs1=x0 offset=0x28
EBREAK
```













