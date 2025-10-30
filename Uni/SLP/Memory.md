## Memory Segmentation in C
1. **Text(Code) Segment**: contains the executable instructions of the program (e.g. program's functions and instructions)
2. **Data Segment**
	1. **`.data` Segment**: holds global and static variables that are initialized by the programmer
	2. **`.bss` Segment**: contains global and static variables that are uninitialized or initialized to zero
		- `.bss` is derived from the older assembler keyword "block started by symbol"
		- Primary purpose: ***memory optimization***
		- does not consume space in the executable file, which keeps the file size small
			- At runtime, the OS allocates the necessary memory for the segment and initialize it to zero
> Variables in `.data` and `.bss` are _static storage duration_, meaning they are allocated once, when the program starts. They remain in memory until the program terminates and ***Their addresses never change***
4. **Heap**: Dynamic memory allocated during program execution
5. **Stack**: used for local variables and function call management
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


## Virtual Memory
- a memor management technique used by the operating systems to give a large/ continuous block of memory to applications, even if the physical memory (RAM) is limited

### Objectives of Virtual Memory
- A program doesn’t need to be fully loaded in memory to run. Only the needed parts are loaded.
- Programs can be bigger than the physical memory available in the system.
- Virtual memory creates the illusion of a large memory, even if the actual memory (RAM) is small.
- It uses both **RAM** and **disk storage** to manage memory, loading only parts of programs into RAM as needed.
- This allows the system to run more programs at once and manage memory more efficiently.

### How Virtual Memory works?
![[Virtual Memory.png]]
- When an program is executed/ an app runs, there is a piece of hardware in the CPU, called the ***MMU (Memory Managment Unit)*** that **maps virtual address of the app to actual physical address**
	- When app request something from **virtual address**, the **MMU** will find the page for that **virtual address** and redirects to the physical address of that paritcular page (frame)
- Even though the memory of an app might start at address 0, when it is mapped to actual physical memory, it could be a completely different one, to anywhere that OS wants
- From the image above, we can see that:
	- App 2 starts at its zero but is mapped to 7864320
	- App 1 is partitioned into 2 parts, one before app 2's memory, one after app 2's memory
	- But they aren't aware of these facts, all they is that in their own virtual address spaces their memory is continuous

#### Address Translation
![[Address Translation.png]]
- For 4KB = 4096 Byte Page, it can store 4096 individually addressable byte. That means the page covers 4096 unique byte positions, each identified by a 12-bit offset (because $2^{12} = 4096$)
	- 12-bit offsets will be copied directly from Virtual Memory to Physical Address
	- The other 20-bit is the **page address** which used in the page table to fetch the upper part of the physical address

### Page and Frame:
- Page: a fixed size block of data in virtual memory
- Frame: a fixed size block of physical memory in RAM where these pages are loaded

- Analogy: 
	- Page is like a piece of a puzzle (virtual memory end)
	- Frame is like a spot where a piece fits on the board (physical memory)
![[Paging.png]]
> Most moderns OS use 4KB size of pages and frames, but smaller and larger size can be used

### Virtual Address Space (VAS) of a process
- consists of all the available memory addresses that this process can refer to
- splits into 2 regions:
	- Kernel Space Virtual Addresses
	- User Space Virtual Addresses
		- contain different types of segments that contain the processes' code, data and dependencies

## `mmap()`
- system call == we ask kernel directly
- `addr` is `NULL` -> kernel chooses the address at which to create the mapping; this is the most portable method of creating a new mapping

## File Descriptor
- a small, non-negative integer returned by system calls like `open()`, `socket()`, `pipe()`
- Each process has file descriptor table which maps unsigned integers to kernel objects. When a process starts, the kernel creates a file descriptor table for it
##  Address Space Layout Randomization (ASLR)
- `mmap`

