- `-static`
## Question 32
### Answer:
- Program binary size is significantly affected by a global int array (by roughly the size of the array) when the array is initialized using a designated initializer, initializing the array with something other than zero

### Why?
- When a global int array of size 1000 is initialized with a desiginated intializer as:
	- `0` -> the array goes into the `.bss` section, which only reserves space (no actual bytes stored in the binary)
	- `5` and the rest filled with zeros -> the array is stored in the `.data` section and 4000 bytes will be added to the program binary size

### Inspection:
- Before & After compiling the program, we can inspect with the following ways:
```bash
size main
```

```bash
readelf -a main | less
```

```
ls -lh main
```

### Outputs: (watch `data` and `bss` segments)
- Without any global array
```
text    data     bss     dec     hex filename
1340     544      8    5916    171c main
```
- With `int array_b[1000] = {0}`
```
text    data     bss     dec     hex filename
1340     544    4032    5916    171c main
```
- With `int array_a[1000] = {1}`
```
text    data     bss     dec     hex filename
1340    4560       8    5908    1714 main
```
## Question 33
- Just like question 32, when a variable is assigned with key `static`, it implicitly turns the variable to a global one. So their behavior is pretty much the same as a global variable. The only difference is that while global variable is accessible to all source files, `static` variable is only accessible within the same source file
	- So if we use a designiated initializer as:
		- null terminating character -> the memory will be stored in `.bss` segment
		- not-null terminating character -> the memory will be stored in the `.data` segment

- Same with `memset()`: Even though the array is zero-initialized during program startup and its memory is stored in `.bss` , `memset()` would only be executed at runtime and doesn't change memory segment of the array
-> doesn't affect the program binary size

- Declared with `register` keyword: each variable can only have 1 storage class. `register` and `static` are 2 different storage classes. They can't coexist

## Question 35:
- Question: **I added the declaration of a large global variable and initialized it with a designated initializer to a non-zero value. Will the program binary size increase by roughly the size of the large global variable?**

## Answer:
- Yes. Just like previous questions had been shown, when initializing a global array with huge size and initialized it with non-zero value. The size of the array will actually be "taken" on the `.data` segment, which proportionally affect the program binary size

## Question 37:
- Question: **With 8GB of available RAM left, I try to allocate 512MB of memory with a single malloc call. Can you guess what the malloc call returned?**

### Solution and Strategy:
- For this question, I wrote a C program that simulates the memory exhaustion that an amount of memory such that there are only 8GB left in my memory
- The intial condition of this simulation: (takes `< 2.20GB/30.6GB)
	- Vscode
	- Terminal running htop
- I allocated 20GB for a char array during runtime and `memset` all 20GB with non-null-terminating values
- Then I allocated 512 MB, then print out the address of that allocated memory

### Trial-and-Error:
- Before the successful aforementioned approach, I tried initializing a global char array with a designated initializer both zero and non-zero and it led to **Segmentation Fault**

#### Why?
When declaring
```C
char array[20UL * 1024UL * 1024UL * 1024UL];
```
- This reserves 20GB of virtual address space for this variabl. When the program runs, the kernal loader will map 20GB from the virtual address space to the actual address space. But if the OS cannot satify that mapping, the process will die immediately with segmentation fault

## Question 39
- Question: **With 4GB of available RAM left, I try to allocate 64KB of memory with a single malloc call. Can you guess what the malloc call returned?**

### Solution:
- Just like question 37, I simulated a C program that exhausts the memory such that there are around 4GB of Ram left
- The condition for the simulation is:
	- Terminal
	- Google Chrome opened with 1 Tab of SNP Test System
	- Vscode
- They take around 3.05GB of RAM
- When I ran the program, it waited at `getChar()` and the RAM then was 26.6/30.6GB
- I ran the program many times and the addresses of 64KB of allocated memory started with `0x5....`

##  Question 40
- Question: **I checked the RAM usage during execution. Then I declared an additional large global variable, recompiled the program and executed it again. Now I can conclude that ...**

### Solution:
- There would 2 scenarios for this question, which are with and without designated initializer/ zero initializer
- Assume I want to create a global char array of size 10GB
	- With designated initializer `char array[TEN_GB] = {'A'}`, during the compilation, the RAM usage would soak up proportionally with the size of the array but then it fails with the following messages:
```
/usr/lib/gcc/x86_64-linux-gnu/14/crtbeginS.o: in function `deregister_tm_clones':
crtstuff.c:(.text+0x3): relocation truncated to fit: R_X86_64_PC32 against `.tm_clone_table'
crtstuff.c:(.text+0xa): relocation truncated to fit: R_X86_64_PC32 against symbol `__TMC_END__' defined in .data section in main
/usr/lib/gcc/x86_64-linux-gnu/14/crtbeginS.o: in function `register_tm_clones':
crtstuff.c:(.text+0x33): relocation truncated to fit: R_X86_64_PC32 against `.tm_clone_table'
crtstuff.c:(.text+0x3a): relocation truncated to fit: R_X86_64_PC32 against symbol `__TMC_END__' defined in .data section in main
/usr/lib/gcc/x86_64-linux-gnu/14/crtbeginS.o: in function `__do_global_dtors_aux':
crtstuff.c:(.text+0x76): relocation truncated to fit: R_X86_64_PC32 against `.bss'
crtstuff.c:(.text+0x9e): relocation truncated to fit: R_X86_64_PC32 against `.bss'
collect2: error: ld returned 1 exit status
```
- The reason is basically I declared very large global variable

	- Without designated initializer `char array[TEN_GB];`, it compiles successfully immediately and the RAM usage stays the same during compilation and execution

## Question 43 + 44

## Question 45
- Quesstion: **I have added a new function definition to my program. Which are closer to the address of my new function: Global uninitialized variables, or global initialized variables?**

### Solution:

## Question 46
- Question: 

## Question 47
- Question: **If I allocate an array with malloc and initialize it using a designated initializer, does the program binary size increase by approximately the size of the array?**

## Solution:
- First of all, designated intializer is meant for compile time, which means it is used for **statically/globally declared arrays or structs**; whereas `malloc` is executed at runtime
- Secondly, even if we don't use something other than _designated initializer_, `malloc()` will only ask the OS for a block of memory on its process's virtual address space, which does not take any memory in physical RAM

## Question 48
- Question: **When does malloc return 0?**

### Solution:
- `malloc()` returns `NULL/0` when the allocation for requested memory fails
- `malloc()` requests more address space from the kernel and the kernel can fail, because no more virtual address space is available 

## Question 49:
- Question: **If I call `char* ptr = malloc(105*1024*1024);`, does my program consume about 105MB more physical memory?**

### Solution:
#### 1. print the returned pointer with `printf("%p", ptr)
- no, it will not take up ram
#### 2. Declaring pointer with `register` keyword
- no, because even we put the `register` keyword in the declaration, it only means that "a register can have the address of a memory location"

## Question 50:
- Question: **I have a program where I used a lot of malloc-calls and accessed all allocated malloc'd heap memory. However, I forgot to call free before the program exits. What happens to the allocated memory, when the program exits?**

### Solution:


## Question 55:
- Question: **I execute this simple program over and over again.**
```C
#include "stdio.h"
int main()
{
    printf("I love SLP!");  
}
```
**Will the (virtual) address of the function `printf` change?**

## Solution: 
- In my program, store the `printf` statement in a variable and run the program multiple times. Apparently, every run, it gives me different addresses of the `printf` statement
- Then I check if ASLR is enable by `cat /proc/sys/kernel/randomize_va_space`
	- I changed the the level of randomization to 0 (no randomization), then I recompiled and reran the program many times and it gives them same address
- When I disable KASLR during boot by adding `nokaslr` in `/proc/cmdline` and repeated the same process as with `aslr`
	- The addresses of multiple runs differed from each other
![[nokaslr.png]]

## Question 60:
- Question: **I have a program with two running threads. Each thread has its own stack. We learned that a stack grows from upper addresses downwards, but can a stack really grow?**

### Solution:
- "Stack grows" means program calls more functions, pushing return addresses, the stack pointer moves downwards in the virtual address space
- `RLMIT_STACK` - maximum size of the process stack (bytes)
- From `man 3 pthread_attr_setstacksize`
	- A thread's stack size is fixed at the time of the thread creation
	- Only the main thread can dynamically its stack
## Question 62:
- Question: **A thread tries to access the stack of another thread. The program crashed. This happened because ...**

### Solution: 
- When thread  A initializes a its stack, and thread B comes and try to access a variable's address and modify it on its stack. It could be the case that thread A has terminated and thread B has already got the address of a stack variable of thread A, but thread address is not invalid
- Thread B tries to dereference it to write something -> Segfault

## Question 66
- Question: **Daniel is implementing some algorithms, to refresh his knowledge. Unfortunately, his program crashed. For debugging purposes, he prints some addresses and the last one he sees is:**

**Address: 0x7ffd357a2610**

**Based on this output and the address, what do you tell Daniel what the most likely reason is why his program crashed?**

### Solution: 
- Looking at the address `0x7ffd35a2610`, especially the part `0x7ffd..`, I can tell that this address lies pretty on the stack -> the problem must happen on the stack
- If it led to kernel page fault, the address would be higher
- If it led to code page fault (meaning happened in the code segment), the address would be much lower
- If it led segfault because NULL pointer is dereferenced, the address would be `0x000...`

## Question 70
- Question: **For a large computation on a modern Ubuntu Linux machine, I need a 256MB array in one function. Which types of variables can be that large without any workarounds or additional settings?**

## Trial and Error:
#### 1. Local Variable
- I tried declaring and initializing an function local array of size 256MB. That means this function is initialized on the stack
- Because the stack of a process in my machine is 8MB, 256MB cause Segmentation Fault + Stack Overflow

#### 2. Global Variable + Static Local Variable
- Without designated initializer, it will only tell the OS to reserve 256MB in process's virtual address space and truly acquire that memory on RAM when it starts accessing it
- With designated initializer, it will acquire the memory on RAM immediately

#### 3. Shared memory


## Question 74
- Question: **I ran a program that has a global variable located at 0x56e5507b1040. However, the result of a calculation using this variable is not as I expected. But then I remembered that there is a second program running in the background also writing to this address. What should I do about this?**

### Solution:
- 2 programs = 2 processes -> Each process runs in its own virutal  address space. That means, the same address of process A and process B are not referring to the same physical RAM
	- Each process has its own page tables 
- So let's say one process tries to modify address which he thinks that he's modifying from another process, then he will basically simply modify whatever physical frame its own address maps to


## Question 75
- Question: **My program has two threads that perform an intensive computation. However, the result is not correct. Upon further investigation, I noticed I made a mistake, and both threads save temporary results in the same global integer variable. What can I do to fix this mistake?**

### Solution:
- Context:
	- 2 threads performing an intensive computation
	- both threads save temporary results in the same global integer variable (shared resource) -> Race condtion
- Recall:
	- Each thread has its own:
		- Stack
		- Register Sets
		- Thread-Local Storage
- The idea is fairly simple, for global/ static variables, we want to make them to local to each thread by either adding
	- `_Thread_local` to the shared resource
	- Or before doing any operations on the shared resource, we copy it to local variable within the routine of the thread and modify that variable


## Question 78
- Question: **Which of these access combinations could yield a segmentation fault?**
	1. Writing to a pointer that has been passed to `free()` before the access.
	2. Writing to a function address.
	3. None of the other answers are correct.
	4. Executing something on the stack.
	5. Writing to memory where we called `munmap()` on before.

### Solution:
- I tested each scenario
1. **Writing to a pointer that has been passed to `free()` before the access**
	- Result: Run with `valgrind` and it yield `Invalid write of size ...`, in this case I was lucky that segfault wasn't yielded
2. **Writing to a function address**
	- The function address lies in the Text Segment where it is read-only and executable
3. **Writing to memory where we call `munmap()`**
	- After `mummap()` was executed, the mapping between pages on virtual memory and physical memory/ files disappeared
	- If we try to access that memory
		- First of all, page-table will skimmed through and see no mapping
		- A page fault will be raised afterwards
		- The kernel's page runs and sees there is no mapping between that virtual address to any physical address -> The fault is not solvable
	-> Segfault
4. **Executing Something on the Stack**
- What "executing something on the stack" means?
	- The CPU's instruction pointer jumps to an address inside the stack region and tries to fetch instructions from there -> the CPU treats your stack data as machine code -> data on the stack becomes instructions


```
7fffa4060000-7fffa4082000 rw-p 00000000 00:00 0           [stack]
```
- As you can see, stack is read-only and write-only and non-executable

## Question 79
- Question: **How big (in bytes) is a (small) page on a modern x86_64 computer?**

### Solution: 
- On modern x86_64 (64-bit Intel, AMD) systems, the default page size 4KB (4096 Bytes). That means the MMU divides virtual memory into pages of 4096 Bytes that are mapped to physical frame in RAM/ disk

#### How to check:
```bash
getconf PAGESIZE
getconf PAGE_SIZE
```

## Question 84
- Question: **I have written a program which allocates and accessses a lot of heap memory, such that it consumes half of my RAM at the moment I call `fork()`. What happens after I call `fork()`?**

### Solution + Observation:
- with `fork()` it creatd a (new) child process that had the same memory layout, same page tables as the parent process. But the kernel does not copy all the memory immediately
- Instead, it uses a [[1 - Multithreading#Copy-On-Write (COW)| Copy_On_Write]], in which shared pages between parent and child is marked as **copy-on-write**
	- That means: when either process writes to one of those pages, the kernel allocates new physical page, copy the original data + updates, update that process's page table entry to point to the new page
-> Only the modified pages are duplicated

### What if you write to memory after `fork()`
- Now physical usage will increase, because every modified page gets duplicated

## Question 85
- Question: **Memory allocated with `malloc()` ... in comparison to memory allocated with `mmap()`.**
	1. is not stored on the hard drive  
	2. does not necessarily yield a segmentation fault when accessed after de-allocating the memory  
	3. was not allocated with the help of the operating system  
	4. is able to be freed with free()  
	5. is never shared between processes  
	6. is not handled by the operating system  
	7. None of the other answers are correct.

### Solution:
- `malloc()`
	- after `free()`, the memory returns to heap, but not umapped, so the heap pages remain valid virtual memory in your process, the memory may still contain the old data.
	- The OS doesn't know you freed it, so segfault
- `mmap()`
	- after `munmap()`, that virtual range has no valid mapping anymore.
	- So when CPU later tries to access that address:
		- The MMU looks up the virtual page in the page tables -> no valid entry -> page fault -> The kernel sees it's not mapped -> Definite SEGFault

## Question 86
- Question: **I defined a variable on the stack as const. Because it is defined as const, ...**
	1. None of the other answers are correct.  
	2.  it is stored in read-only physical memory.  
	3. it is stored in read-only virtual memory.  
	4. the CPU prevents the user from writing to it during runtime.  
	5. we can not change its value with additional code.  
	6. it never goes out-of-scope.

## Solution:
- A const variable on the stack can go out of scope in the following scenarios:
	- when that varible is initiazlied inside a function and that function returns 
	- The variable is initialized inside a self-made brackets in `main()`
- The `const` keyword only tells the compiler that it is doesn't intend to be modified, the compiler enforces this at compile time, not at runtime. 
	- Even though the variable is marked `const`, it is still writable by accessing its memory address and modify there
- Ultimately, the stack is not only readable, but also writable
	- We can check that with `cat /proc/$(pidof main)/maps and you get
```
7ffcda5bb000-7ffcda5dd000 rw-p 00000000 00:00 0         [stack]
```