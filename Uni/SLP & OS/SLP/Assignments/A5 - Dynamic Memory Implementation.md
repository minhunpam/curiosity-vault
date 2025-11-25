## Overview:
- When we allocate, let's say100 Bytes, the allocated Bytes under the hood shouldn't be precisely, says 110 Bytes. But it shouldn't overly much more than the `sbrk()`

## Where & How to start
- It is recommended to start off with a simple `malloc(...)` and `free(...)`
- The task of the operator new is much, much simpler than the `malloc(...)` task

## Requirements
### Mandatory
-  The interface of your implementation is required to conform to the POSIX standard
	- `void* malloc(size_t size)` allocates `size` Bytes and returns a pointer to the allocated memory
	- The memory is not initialized
	- If `size` is 0, then `malloc` returns a unique pointer value that can later be successfully passed to `free()`
		- This means: `malloc(0)` may return some pointer (not NULL)
			- Doesn't refer to any valid memory block you can use
			- A special pointer that you must later free if you want to avoid memory leaks
			- Is unique in the sense that it is distinct from other allocated pointers

- ***Reduce the heap size whenver it makes sense*** (for example: when all the malloc'd data has been freed)
	- Note: this is not necessarily standard `malloc(...)` behaviour
	- Can decide when exactly you can reduce the heap size
- Do not trust user pointers
	- Make sure your functions never cause segfault if you could prevent it
- Implement 2 functions
	- `snp::Memory::total_used_memory(...)` - returns the total amount of currently allocated memory (`is_free = false`)
		- Simple:
			- Traverse through the linked list, for each node we have the allocated memory size = `sizeof(Node) + node->size`
		- ***But I think, we can somehow fetch this data in constant time such as storing in each node linked list current state***
	- `snp::Memory::free_block_info(...)`
		- takes a parameter `int type`
		- When the type is  a non-zero value, return the size of the first free block
		- When type is 0, return the starting address of the first free block
		- If no free block exists, return 0
		- **Idea**:
			- Traverse through the linked list and get the first free block and process with respect to the parameter
			

### Additional
#### Malloc, Calloc, Free
- ***Reduce the number of syscalls where possible***
	- allocating `100KB` should take more than 1 but no more than 30 syscalls, regardless of the number of `malloc(...)` calls
	-  ***You are not allowed to call `sbrk(0)` more than once during program execution***
		- `sbrk(0)` -> get the current program break
		-  ***Can assume `sbrk(...)` is only called in your malloc/free functions***		
	- When happens internally when you call `sbrk(...)`?
	- Does it make sense to call `sbrk(1)`? == increment only 1 Bytes
		- No. Instead, everytime, we want to extend the heap, we want to double the size
	- Use `strace` to examine the syscalls a process makes
- Detect memory corruption errors
	- Buffer overruns/memory corruption
		- Buffer overruns: When you allocate 10 Bytes, but write to that memory slot 20 Bytes
			- Detection: Since malloc internallly can't check if user writes to invalid pointers, for the next `malloc()` run we have to check that and exit
			- The overwritten data would lie inside the memory for Metadata
	- invalid `free(...)`
	- Double `free(...)`
	- Handle out of memory correctly
- In case you run ***out of memory**  returns NULL

## `malloc()`
### Idea
- For each time we call `malloc`

## Possible Errors:
- Double free
- 

## `calloc()`
- 

## `free()`
### Idea
- For address of node that are not the last node, then we will set the node's `isFree = true` so that any newly allocated node can compare it wanted size
- For address of node that is the last node, we have to `sbrk()` that
- Keyword:
	- Coalescing
	- Heap Trimming