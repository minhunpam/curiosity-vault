## Virtual Memory
- a memory management technique used by the operating systems to give a large/ continuous block of memory to applications, even if the physical memory (RAM) is limited

### Objectives of Virtual Memory
- A program doesn’t need to be fully loaded in memory to run. Only the needed parts are loaded.
- Programs can be bigger than the physical memory available in the system.
- Virtual memory creates the illusion of a large memory, even if the actual memory (RAM) is small.
- It uses both **RAM** and **disk storage** to manage memory, loading only parts of programs into RAM as needed.
- This allows the system to run more programs at once and manage memory more efficiently.

### How Virtual Memory works?
![[Virtual Memory.png]]
- When an program is executed/ an app runs, there is a piece of hardware in the CPU, called the ***MMU (Memory Managment Unit)*** that **maps virtual address of the app to actual physical address**
	- When app request something from **virtual address**, the **MMU** will find the page for that **virtual address** via page table
	- And redirects to the physical address of that paritcular page (frame)
	
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

### Swap Space (Paging Space/ Swap File)
- dedicated area on the hard disk used by the OS as an **extension of physical RAM**
![[lazy-swapping.jpg]]
### Working of Swap Space
1. When RAM is full, the OS selects some memory **pages that are inactive or least recently used.**
2. These pages are written from RAM -> swap space on the disk.
3. When those pages are needed again, they are read back from the swap space into RAM.
4. This process of moving pages between RAM and disk is called **swapping or paging**.


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

## Thread-local Storage
- a memory management method that uses static or global memory local to a thread
- The concept allows storage of data that appears to be global in a system with seperate threads
### In C
- denoted with `_Thread_local`
	- global/ static variables will become local to the thread in which they are declared
	- These variables are allocated when the thread begins and de-allocated when the thread ends
	- Every thread **has its own copy of the variable with `_Thread_local` keyword**
