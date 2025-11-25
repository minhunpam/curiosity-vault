
- Implement a simple starting prototype
- Write tests for your implementation
- Debug and improve your implementation
## `brk()` and `sbrk()`
- `brk()`
	- Sets the program break (the end of the heap) to an absolute address
	```c
	int brk(void* addr);
	```
- `sbrk()`
	- Moves the program break by an offset (positive or negative)
	```C
	void* sbrk(intptr_t increment)
	```
	

## System-level Code
- Some errors won't be caused by your code directly user input
- You have tto manage memeory manually
- Some library functions may not work as expected

## Debugging
- Use utility functions in your tests
	- Check after each mallocation and deallocation if the metadata is correct
- Use `assert()` or `if(error) exit(-1)`
- Use gdb

## Testing
- Write many SMALL test cases
- Each test case checks for 1 specific siutation
- Test the same scenario in different variants
	- Switch allocation/ free order
	- Different sises

###  Negative tests
- Even if your does not crash there might be hidden bugs
- should cause errors

- We have to keep track which address is valid/ invalid

## Memory Allocator Basics
### Metadata
- How do you know which blocks are free and which ones are used?
- How do you know the size and location of blocks?
-> Store metadata