- The software

## Programming in Assembly
- Sum up 10 numbers and print the result
```assembly
.org 0x00
ADD x1, x0, x0 # clear x1

#1
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#2
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#3
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#4
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#5
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#6
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#7
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#8
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#9
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
#10
LW x2, 07fc(x0) # load input
ADD x1, x1, x2  # x1 += input
```

### Loops
- Start with the code for one iteration
- Add loop variables
- Increment the counter
- Branch to the start of the loop
```assembly
.org 0x00
	ADD x1, x0, x0 # clear x1
	ADD x3, x0, x0 # clear counter
	ADDI x4, x0, 10 # iteration count
	
	LW x2, 0x7fc(x0) # load input
	ADD x1, x1, x2 # x1 += input
	
	ADDI x3, x3, 1 # counter ++
	BLT x3, x4, -12 # if (counter < 10) loop
	
	SW x1, 0x7fc(x0) # output sum # Store data to an absolute address
EBREAK
```
- On line `BLT x3, x4, -12`, it is not really great to count offsets for us, so we will let the compiler does the loop thing with SYMBOLS
	- We label memory addresses
	- Each addresses we label is assigned a symbol ("a name")
	- When programming, we can replace memory addresses by symbols to simplify  the complexity of programming

```assembly

.org 0x00
	ADD x1, x0, x0 # clear x1
	ADD x3, x0, x0 # clear counter
	ADDI x4, x0, 10 # iteration count
loop:	
	LW x2, 0x7fc(x0) # load input
	ADD x1, x1, x2 # x1 += input
	
	ADDI x3, x3, 1 # counter ++
	BLT x3, x4, loop: # if (counter < 10) loop
	
	SW x1, 0x7fc(x0) # output sum # Store data to an absolute address
EBREAK

```
## Important Steps for Transformation from C to ASM
- Transform all `for/while` into conditional `goto` statements (if + goto label)
- Resolve complex conditional statements and computational statements by using temporary  variables
	- ASM instructions can only handle 2 operands
- Ensure the correct handling of the `else` branch when resolving`if` statements (if + goto label) statements
- Make pointer  arithmetic of arrays explicit

## Functions calls
- Basic Idea:
	- Partitioning of code into reusable functions
	- Functions can call other functions arbitrarily (Nested function call, recursive function calls)
- Interface:
	- The function takes input arguments
	- The function provides a return value as output
- A function call is not simply a branch instruction
- Whenever there is a function call, we also need to store the return address
	- The return address is a mandatory parameter to every function

### Realizing functions calls and returns on RISC-V
- RISC-V has 2 instructions to perform a "jump and link"
	- **[[RISC-V Basics#JAL/JALR|JAL (Jump And Link)]]**: `JAL rd, offset`
		- Jump relative to current PC
		- The jump destination is PC + offset
		- Upon the jump (PC + 4) is stored in register `rd`
	- **[[RISC-V Basics#JAL/JALR|JALR (Jump And Link Registers)]]**: `JALR rd, offset(rs)`
		- Jump to address (register content from rs) + offset
		- Upon the jump (PC + 4) is stored in register `rd`
- ***Problem: JAL and JALR need a register for storing the return address***
	- We could use different registers for each function call. However, we would quickly run out of registers
	- We need a data structure in memory to take care of this

## Stack
### Characteristics:
- 2 Operations:
	- "Push": places an element on the stack
	- "Pop": receives an element from the stack
- FILO
- Typically "grows" from high to low address
- The stack is a continuous section in memory
- The ***stack pointer*** (`sp`) points to the top of the stack (TOS)

![[CON - Stack.png]]

### What is stack frame?
- A block of memory on the stack that a function uses to store everything it needs while it is running
- It is created when a function is called and destroyed when the function returns
- Each stack frame maintains the **Stack Pointer (SP)** and **Frame Pointer (FP)**
	- They always point to the top of the stack
![[CON - Stack frame example.png]]
### Implementing a Stack with RISC-V
- Initialize a stack pointer 
	- Set starting point
- Push value
	- Expand stack by 4
	- Copy value from register to top of the stack
- Pop value
	- Copy value from top of the stack to destination register
	- Decrease stack by 4

### Example:
![[CON -  stack example.png]]

## Register Usage in Subroutines
- We can use a stack to store return addresses
- In fact, the stack can be used as a storage for any register
- Assume you want to use register `x1`,  but it currently stores another value that is needed later on
	- Push `x1` to the stack
	- Use `x1`
	- Restore `x1` by popping the content from the stack
	-> This is called "register spilling"

Idea:
- We can use the stack to store and restore register states when entering/ exiting function calls
- Every function can use the CPU registers as needed

## Calling Convention
- Calling Convention is a set of rules that defines how the caller and callee must cooperate during a function call
- Each platform has a specific calling convention
- Calling convention defines the relation between
	- ***The caller - the part of the program doing a call to a subroutine***
	- ***The calee - the subroutine that is called***
- It defines:
	- How are arguments passed between caller and callee?
	- How are values returned from the callee to the caller?
	- Who takes care of the stacking of which registers?

### RISC-V registers
![[CON - Calling Convention.png]]
- Summary:
	- Saved by Caller:
		- `ra` - (return address)
		- `a0 - a1` (arguments/ return values)
		- `a2 - a7` (arguments)
		- `t0 - t6` (temporary registers)
			- Caller-saved = the Caller is responsible for saving t0 - t6 if it wants to keep them, but the callee is allowed to overwrite `t0-t6` freely. That when entering a subroutine, using directly the temporaries is not recommended, but we have to load word from function arguments into temporaries before using it
	- Save by callee:
		- `fp` - frame pointer
		- `sp` -  stack pointer
		- `s1 - s11` - saved registers

### Switching from HW to SW view
- All subsequents assembler examples will be written using the ***software ABI conventions***, with no `x...` registers anymore
- In hardware this does not change anything - it is just the naming

## Code parts of a Subroutine
- Important code parts for the handling of registers, local variables and arguments are
	- ***Function Prologue ("Set-up")*** - the first instructions of a subroutine
	- ***Neighborhood of a nested call*** - before and after call
	- ***Epilogue ("Clean-up")*** - the last instructions of a subroutine

## Frame Pointer
- If there are too many arguments to fit them into the registers, the additional parameters are passed via the stack
- In order to facilitate the access to these arguments, we introduce the ***Frame Pointer***
	- ***The frame pointer stores the value of the stack pointer upon function entry***
		- The frame pointer always points to the last element that the caller has put on the stack before jumping to the callee

- In case there are parameters passed via the stack from the caller to the callee, it holds that
	- FP: points to the first argument on the stack (the first argument is at the lowest stack address)
	- FP + 4: points to the second argument on the stack
	- FP -  4: this is the first element that is placed on the stack by the callee - in our examples, this is typically the return address (`ra`)
- The frame pointer is set and saved by the callee -> If a callee wants to use a frame pointer, the callee needs to
	- Stack the current framepointer (fp)
	- Set the fp to its stack frame (the value of `sp` upon function entry)

## Local Variables
- Whenever a function requires local variables, these variables are also stored on the stack

## Call by Value vs. Call by Reference
- There are 2 important ways of passing arguments to a function
### Call by Value
- The values of the arguments are provided in the registers `a0-a7` and the stack

### Call by Reference
- Instead of values, pointers are passed to the funciton (they point for example to variables of the stack frame of the caller)

## Memory Layout of Stack frames
![[CON - memory layout of stack frame.png]]






