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


