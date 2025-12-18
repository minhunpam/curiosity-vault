```bash
objdump -d <executable>
```

```
Caller:
main: 
  # ...
  call foo
  # ...
  
Callee:
foo:
  # do stuff ...
  ret
```
- `call` instruction pushes return address onto stack and jumps to target
- `ret` instruction pops return address from stack and jumps back

- A calling convention defines the interaction between functions on the level of CPU-instructions
	- Function parameters
	- Return values
	- Registers that need to be saved/ restored across function calls
- Calling conventions are not only relevant within a single binary. All interfaces between binary modules need to conform to a common interface to be compatible
	- Object files that are linked together at compile time
	- Dynamically loaded libraries (e.g. `libc`)
	-> Defined as part of an ABI (Application Binary Interface)
	- A complete ABI also defines the executable format (e.g. ELF), instruction set
- The used ABI/ Calling convetion depends on
	- CPU architecture
	- Operatin  system
	- Compiler

## How do 32-bit funtions work?
- We need stack for function calls
- Register `esp` points to the top of the stack
- Assembly instructions `push` and `pop` use and modify `esp`

## How does a system call work?
- Put all parametes into registers
- Request an interrupt
- The interrupt 

- `1e fa` - function prologue
- `89` - `mov` instruction
- `rdx` - 64 bit
- `ecx` - 32 bit

- `rdi` - 1. argument
- `rsi` - 2. argument


## Inline Assembler
- Sometimes it is required to use assembler within a C program
	- e.g. for optimization of certain computational tasks
	- or tasks, which require low level instructions
- We use the GNU inline assembler
	- Allows to inline assembler code in C programs
	- Uses AT&T syntax

### Revisiting assembler
- Assembler is a low level language
	- easily convertable to machine code
- Relies on a "small set" of instructions
	- Arithmetic/ logical: `add`, `sub`, `and`, ...
	- Load/ store/ move: `ld`, `st`, `mov`, ...
	- Comparisons: `cmp`
	- Jumps/ conditional jumps: `jmp`, `je`, `jne`, ...
- In contrast to high-level languages
	-  Very exact specification of what the CPU should do

### AT&T Syntax
- `instruction source, destionation`
	- opposite to Intel syntax
- Instructions are suffixed with
	- `b`... byte
	- `w`... word,
	- `l`... long word
	- `q`... quad word
- Registers prefixed with `%`
	- Thus, no confusion with C symbols
	- Example: `mov %ebx, %eax`
- Immediate operands prefixed with `$`
	- Example: `movl $5, %eax`
- Accessing in-memory content
	- `movl foo, %eax` -> contents of variable `foo` put into register `eax`
	- `movl $foo, %eax` -> address of variable `foo` put int register `eax`
	- `movl (%rax), %eax` -> dereference pointer stored in `rax` and put into register `eax`
		- For example:
			- `rax = 0x400100`
			

| Address  | Value |
| -------- | ----- |
| 0x400100 | 0x11  |
| 0x400101 | 0x22  |
| 0x400102 | 0x33  |
| 0x400103 | 0x44  |

- These 4 bytes (litte-endian) form the 32-bit value: `0x44332211`
- Now the instruction: `movl (%rax) %eax`
	- Look inside `rax` -> get `0x400100` -> go to memory `0x400100` -> read 4 bytes -> put those 4 bytes into `eax`
	- `eax = 0x44332211`

#### Addressing in AT&T Syntax
- Addresses calculated as `BASE + (INDEX * SCALE) + DISP`
- `movl $5, %eax`: Move the constant with value 5 to the EAX register
	- Put the number 5 directly into the register EAX
	- `eax = 5`
- `movl $5, (%eax)`: Move the constant 5 to the address EAX points to
	- Take the number 5 and store it into the memory address contained in EAX
	```C
	*(int*)eax = 5;
	```
- `movl $5, 4(%eax)`: Add 4 to the address in EAX and move the constant 5 to this location
	- Take the value 5 and store it into memory at the address (EAX + 4)

### The GNU Inline Assembler
- Including assembler snippets in a C program:
```C
asm volatile("movl %ebx, %eax");
```

- Accessing global C variables:
```C
int cVar = 7;
asm volatile("movl cVar, %eax");
```

### Define the interface information
```C
int sum, op1 = 5, op2 = 3;
asm volatile("addl %2, %1\n\t"
			"movl %1 %0\n\t"
			: "=r" (sum)              // output operands
			: "r" (op1), "r" (op2)    // input operands
			:                         // clobbered operands
);
```
- `"=r" (sum)` - the operand is written to `sum`
	- `=` or the equation signs indicates that your assembly code doesn't care about the initial value of the mapped variable
- `"r" (op1)` - a general register is used to store `op1`
- Inputs and outputs accessed via `%0, %1, %2,...`
	- **Operand numbering rule (very important)**
		1. All outputs first
		2. Then all inputs
		3. Left to right
- ***If registers are directly modified: prefix register with `%%` and add to clobber list***

#### True understanding of `%%`
- When you write inline assembly, 2 different things read your text
1. The C compiler (GCC/ Clang)
2. The assembler (which understands `%rax`, `%rdi` etc.)
- The confusion comes from the fact that both use `%` for different purposes
- To C compiler, `%` means a placeholder marker
```C
asm("mov %0 %1");
```
- `%0` = “replace this with operand 0”
- `%1` = “replace this with operand 1”
But registers also start with `%`
```asm
%rax
%rdi
```
So if we write:
```C
asm("mov %rax, %rdi);
```
- The compiler would think: "Oh! `%r` must be an operand placeholder...but where is the number"
- So we have to signify the compiler somehow that `%` is meant for the registers, by which we add one more `%` to the front

### Constraints
- Define whether an operand is
	- in a register
	- in which kind of register
	- a  memory reference
	- which kind of address
	- an immediate constant
- Constraints are quoted strings inside the outputs and inputs
#### "r" -- general-purpose register
```C
"r"(x)
```
- Meaning: Put `x` in some general-purpose register
- Which one? --- compiler decides

#### "m" --- memory
```C
"m"(x)
```
- Meaning: Use the memory address of `x`

#### "=r" --- output register
```C
"=r"(out)
```
- Meaning: writes to a register and stores the result in `out`
- The `=` means write-only

#### "+r" --- read/write outputs
```C
"+r"(x)
```
- Meaning: `x` is both input and output 

#### "0", "1", ... --- matching constraints
```C
asm("add %1, %0"
	: "=r"(out)
	: "r"(in), "0"(out)
);
```
- `"0"` means same register as `%0`
- Instruction requires same source/destination

#### "g" --- use any general operand: register OR memory OR immediate



## What is so special about `eax` register?
- In x86 System V ABI, `eax` is **the register where functions return integers/ pointers**. The machine code will place `123` in `eax` before `ret`
- `eax` is the fastest register to access

## Function calls in Inline Assembly
- When calling a function in inline asm, we must respect:
	- where arguments go
	- where the return value comes back
	- which registers might get clobbered
- On System V x86-64:
	- 1st integer argument -> `rdi` -> `"D"`
		- Besides, `rdi` ***is used for the exit code because the platform's calling convention says so***
	- 2nd integer argument -> `rsi`-> `"S"`
	- Return value -> `rax`
- The caller uses registers to pass the first 6 arguments to the callee.  Given the arguments in left-to-right order, the order of registers used is: 
	1. `%rdi`, 
	2. `%rsi`, 
	3. `%rdx`, 
	4. `%rcx`, 
	5. `%r8`, 
	6. `%r9`
	Any remaining arguments are passed on the stack in reverse order so that they can be popped off the stack in order.

### Are `edi` and `rdi` the same thing?
- They are different sizes of the same physical register
- On x86-64, each general-purpose register has multiple size aliases:

| Size   | name  |
| ------ | ----- |
| 64-bit | `rdi` |
| 32-bit | `edi` |
| 16-bit | `di`  |
| 8-bit  | `dil` |

### Clobber List
> Any register listed as an output must NOT appear in the clobber list. For example:
```C
: "=a"(a)
```
This asm produces an output in RAX, and after the asm finishes, the value in RAX must be copied into variable `a`. So the compiler already knows: `rax` is written, the final value in `rax` is important,  it must not reuse old assumptions about `rax`. Therefore `rax`is automatically considered clobbered

#### `"memory"` clobber
The **`"memory"` clobber** tells the compiler:
> “This inline asm may read or write **any memory**, not only the variables listed as inputs/outputs.  
> Therefore, the compiler must NOT move or reorder memory operations across this asm.”

- We must use `"memory"` whenever a function is called inside inline asm. Because the called function may read or write to ANY memory. The compiler must assume all memory can change
##### When not to use "memory"
- touches ONLY registers
- has no memory effects
- is a pure computation

#### "cc" in the clobber list
- `cc` = condition codes (CPU flags)
- These include:
	- ZF (zero)
	- SF (sign)
	- CF (carry)
	- OF (overflow)
- Instruction like: `addq %%rbx, %%rax` modifies flags. So we must tell the compiler `"cc"`. Otherwise the compiler might: assume flags are unchanged, reuse flags for later conditionals, produce incorrect code
## named operand
```C
asm(
	"mov %[value], %%rax"
	:
	: [value]"r"(g)
);
```
- When you embed assembly, you often want to refer to operands by name instead of numbers like `%0`, `%1`, `%2`, etc.
- Named operands make asm easier to read and safer
- Inside the string `"mov %[], %%rax"`:
	- `%[value]` expands to whatever register GCC put the variable g into

## What does "mnemonic" mean?
- A human-readable that represents a low-level operation. In assembly language, a mnemonic is the textual instruction name that stands for a machine instruction (binary opcode). They are designed to easy to remember
- For example:
	- Assembly (human-readable)
	```asm
	movq %rax, %rdi
	addl $1, %eax
	callq exit
	```
	- Machine code (what the CPU actually runs)
	```
	48 89 C7
	83 C0 01
	E8 ...
	```
Here:
- `movq`, `addl`, `callq` → **mnemonics**
- `48 89 C7` etc. → **opcodes + operands**

## What is the difference between `mov` and `movq`
👉 **`movq` explicitly means “move a 64-bit (quad-word) value.”**  
👉 **`mov` is a generic mnemonic whose size is inferred from the operands.**


| suffix | meaning | size    |
| ------ | ------- | ------- |
| `movb` | byte    | 8 bits  |
| `movw` | word    | 16 bits |
| `movl` | long    | 32 bits |
| `movq` | quad`   | 64 bits |

## What does `callq` do?
- Call a function using 64-bit return address

- The function-call instruction in x86-64
- At CPU level, it does 2 things:
	1. Pushes the return address onto the stack
	2. Jumps to the target function
- The suffix `q` means quad word --> push an 8-byte (64-bit) return address

| Instruction | Meaning          |
| ----------- | ---------------- |
| `call`      | generic mnemonic |
| `callq`     | 64-bit call      |
## What do `pushq` and `retq` do?
### `rsp` - stack pointer
- points to the top of the stack --> alwasy contains the address of the current top element
- uses `rsp` for:
	- function calles
	- local variables
	- saving registers
	- return addresses
- `pushq %rax` = `rsp = rsp - 8` then `*(uint64_t*)rsp = rax`
- `popq %rax` = `rax = *(uint64_t*)rsp` then `rsp = rsp + 8`
### `rip` - instruction pointer
- holds the address of the next instruction to execute

### `retq`
1. Pop 8 bytes from `[rsp]`
2. Load that values into `rip`
3. jump to the address after the pop
