## Memory Segmentation in C
1. **Text(Code) Segment**: contains the **executable** instructions of the program (e.g. program's functions and instructions)
	- _This segment is usally read-only to prevent accidental modification during execution
	- typically stored in the lower part of memory
	- 
2. **Data Segment**
	1. **`.data` Segment**: holds global and static variables that are initialized by the programmer
	2. **`.bss` Segment**: contains global and static variables that are uninitialized or initialized to zero
		- `.bss` is derived from the older assembler keyword "block started by symbol"
		- Primary purpose: ***memory optimization***
		- does not consume space in the executable file, which keeps the file size small
			- At runtime, the OS allocates the necessary memory for the segment and initialize it to zero
> Variables in `.data` and `.bss` are _static storage duration_, meaning they are allocated once, when the program starts. They remain in memory until the program terminates and ***Their addresses never change***
3. **Heap**: Dynamic memory allocated during program execution
4. **Stack**: used for local variables and function call management
	- Each function call creates a stack frame in this segment
![[Memory-Layout-of-C-Program.webp]]
## Ways to inspect program binary size:
```bash
readelf -a <your_program_exec> | less
```
- `readelf` - tool that reads and displays information from ELF (Executable and Linkable File) binary files
- `-a` - shows everything = `-h -l -S -s -r -d -V`

```
ls -lh <your_program_exec>
```

```bash
size <your_program_exec>
```
- list section sizes and total size of binary files

## Runtime Initialization of variables
```C
#include <stdio.h>
#include <stdlib.h>

int var[10];  /* Uninitialized, so in the .bss segment */

int main()
{
    var[0] = 20; /* Initializing at runtime */
    printf("var[0] = %d\n", var[0]);
    return 0;
}
```
- Even though `var` is initialized at runtime, it still resides in the `.bss` segment, because the inializaton happens at runtime, which does not change the location of `var` in memory, it remains in the `.bss` segment throught the program's execution

## `mmap(void* addr, size_t length, int prot, int flags, int fd, off_t offset)`
- system call == we ask kernel directly
- `addr` is `NULL` -> kernel chooses the address at which to create the mapping; this is the most portable method of creating a new mapping
	- the protections only apply to the current process
	- If another process maps the same file into its virtual memory space, that second process may set different protections


### Memory-mapped files
- File is mapped to a particular region in memory -> the process sets up a pointer to the beginning of that region. The process can deref the pointer for direct access to the contetnts of the file


### What happens when `mmap()` is run
- The kernel creates a **mapping record (Virtual Memory Area/ VMA)** in the process's internal memory map
- Finds a free region in your process's virtual address space and links that region to a "backing object"
	- a file
	- anonymous memory
	- device memory
- Returns a pointer - the starting virtual address of that region

- The kernel only sets up the mapping rule - a promise like: 
> **“If this program ever touches addresses between 0x7f8a0000–0x7f8a1000, those bytes correspond to data from file X offset Y.”**

#### When touching the memory
1. CPU tries to access virtual address `0x7f8a0000`.
2. MMU looks up your **page table**, but finds no physical page yet.
3. Hardware triggers a **page fault** (a trap to the kernel).
4. The kernel checks:
    - “Which mapping (VMA) covers this address?”
    - “Is it file-backed or anonymous?”
5. The kernel **allocates a physical page**:
    - If file-backed: loads that 4KB chunk from the file into RAM.
    - If anonymous: allocates an empty, zeroed page.
6. The kernel **updates the page table** to map that virtual page → physical page.
7. The instruction is retried, and it succeeds.

### `munmap()`
- corresponds to `free()` if `malloc()` uses `mmap()` internally to allocate memory
- `munmap()` could free whole or part of memory

## File Descriptor
- a small, non-negative integer returned by system calls like `open()`, `socket()`, `pipe()`
- Each process has file descriptor table which maps unsigned integers to kernel objects. When a process starts, the kernel creates a file descriptor table for it

## `malloc(size_t size)`
- allocates the `size` bytes and returns a pointer to allocated memory. ***The memory is not initialized***
	- That means if `malloc()` succeeds, it asks the OS for a block of memory of ***virtual address space*** but ***physical RAM frames are not immediately commited***
		- For small allocations, it might come from the heap using `brk()`
		- For lage allocations, the C lib uses `mmap()`
	- Only when your program **writes** to that allocated  memory (touches each page of VAS) does the kernel allocate a real frame of RAM (or swap space) and map it in
- 

## Calling Convention
- `%rdi` - first argument 
- `%rsi` - second argument
- `%rax` - hold the return value of function
-


## Understanding Memory Address
- A memory address is just a number, telling the CPU where a byte or block of data is located in memory
- The memory address in C is printed out in **hexadecimal form**

### The meaning of `0x` prefix
- `0x` just means "the number after this is in hexadecimal"

### Why hexadecimal
- Because memory addresses are 64-bit - that's 8 bytes  -> There are $2^{64}$ possible locations. Writing those in decimal would be unreadable, so hexadecimal is used - every hex digit = 4 bits
- 16 hex digits = 64 bits in total


##  Address Space Layout Randomization (ASLR) & Kernal Address Space Layout Randomization (KASLR)
## ASLR
### Fundamentals

### Verification
- Reading the value of `/proc/sys/kernel/randomize_va_space` file that contains the level of randomization:
	- 0: No randomization
	- 1: Conservative Randomization
		- The shared libraries, stack, `mmap()`, and VDSO page are randomized
	- 2: Complete Randomization

### Disabling ASLR
```bash
echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
```
### Enabling ASLR
```bash
echo 2 | sudo tee /proc/sys/kernel/randomize_va_space
```
