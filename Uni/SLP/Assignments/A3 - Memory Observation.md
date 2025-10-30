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

