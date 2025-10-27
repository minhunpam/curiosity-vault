## Basic Structure of `Makefile` element #Makefile

```
main: main.c
	$(CC) -Wall -Wextra -pedantic -std=c99 -o main main.c
```
- `main` = what we want to build
- `main.c` = what is required to build `main`
- second line specifies the command to run -> actually build `main` out of `main.c`
	- ⚠️ **Make sure to indent the second line with an actual `TAB` character**
	- `$(CC)` = is a **Makefile variable** -> C compiler -> used to abstract away the actual compiler command, usually `gcc` / `clang`
	- `-Wall` = "**all W**arnings" -> gets the compiler to warn you when it sees code in your program that might not technically wrong, but considered bad or questionable usage of the C language
	- `-Wextra` and `-pedantic` = even more warnings
	- `-std=c99` = specifies the exact version of the C language **st**an**d**ard

## Making `Makefile` clean #Makefile 
- To make it easy for scalability and maintenance, we can make aliases for each components of a command:
```
Compiler: CC = clang -> $(CC)
Compiler flags: CFLAGS = -Wall -Wextra -pedantic -g -std=c99 -> $(CFLAGS)
```
- ⚠️ The `$` sign is used to **reference a variable** ("expand the variable")
	- using `$(...)` or `${...}` for clarity and compatibility


## What does `.PHONY` do? #Makefile 
- Make tries to **match targets to filenames**. If a file named, for example `clean` exists in your directory, then:
```
make clean
```
will do nothing

## How does `wildcard` work? #Makefile
```
SRCS := $(wildcard *.cpp) $(wildcard src/*.cpp) $(wildcard src/buttons/*.cpp)
```
- Let's take `$(wildcard *.cpp`) for example:
	- `wildcard` is a built-in Make function - expand patterns like `*.cpp` to a space-separated list of matching files
	- The `$` tells Make: "Evaluate what's inside these parentheses as a function or variable."


## `=` vs `:=` #Makefile 
- `=` :
	- Value is not expanded immediately.
	- Every time you use LHS variable, **Make will re-evaluate** the RHS.
	- Use case: useful when the value depends on things that might change later.
- `:=`:
	- Expanded immediately.
	- All further references to LHS variable just use the computed value.
	- Use case: faster, safer, and often preferred
