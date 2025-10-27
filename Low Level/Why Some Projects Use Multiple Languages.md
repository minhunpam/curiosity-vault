## How does Compiler like GCC operate?
It goes through 4 steps:
1. Pre-processor: `.c` file --> `.i` file
	- Remove comments
	- Expand macros
	- Resolve compilation
	- ⚠️Resolve `#include` (pre-processor replaces the `#include` line with the contents of header files)
2. Compiler: `.i` file --> `.s` file
	* Turn C language code into human-readable Assembly language, which is the an intermediate representation between source code and machine code
3. Assembler: `.s` file --> `.o` file (object file)
	- Turn Assembly code into machine code with 1s and 0s
	- ⚠️ But the object file is runnable, because 1 object file only contains machine code, but for **individual translation units** (1 `.c` /`.cpp` file) and it may reference external symbols (variables/ functions) that are declared but no defined in this file --> Resolve all external resources == going through the same compilation process as before
4. Linker: link multiple object files into 1 executable code
	- There are 2 options for the linking process:
		1. **Static linking**: Copy the needed code from the object file into final executable
		2. **Dynamic linking**: Common libraries are pre-compiled into **dynamic shared libraries**
			- Linux-based: `.so`
			- Window-based: `.dll`
			The linker inserts a reference to the library that contains machine instruction for that function. At runtime, the OS loads the required function into the program's address space

- Using `-save-temps` to compile to expose all intermediate files
- We can even stop the process in intermediate step by using the correct flag in the compile command
- We can start from any phase in the pipeline, such as:
```
clang main.s -o main
```
--> This means we can write our code in Assembly and pass to the pipeline and the linker will take care mixing them together into executable file

## An example: Writing a program to calculate how many primes number between 0 - 1000
- We can write the whole thing in C, but we don't trust the compiler optimization, so we write instead the hard calculation `isPrime` in Assembly and call it from C
- Then we pass both files to the the compiler 

## GCC isn't just a Compiler!
- `GCC` is more likely a toolchain
- A pipeline of tools that are executed in sequence with each stage consumes the output of the previous one
- Each of these tools is pluggable 
- GCC supports C, C++, Object-C++, Fortran, Ada, D and Go
- Originally: `GCC` -> GNU C Compiler
- `GCC`: GNU Compiler Collection

## Assembly is not for everyone!
- What if we implement parts of our project in Fortran
- In this case, we need multiple steps:
	- Compile the `Fortran` file --> Fortran object file
	- Compile the `C` file --> C object file
	- Link both object files into a single executable

--> TURNS OUT, A PROJECT OF MULTIPLE LANGUAGES DOESN'T COME DOWN TO A COMPILER SUITE, BUT THE LINKER

- For a project, it is necessarily blazingly fast, just certain parts. So most of the time developers try to write most of the project in high-level language for convenient development speed. 
- When it comes to optimization and performance, the implementation would be held in low-level language like C

## Having final linking phases, doesn't mean being linked together correctly into 1 executable

- **ABI = Application Binary Interface** - defines how different components of different binary code communicate with one another through the hardware