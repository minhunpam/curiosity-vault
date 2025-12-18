## `qsort_transformed.c`
### `swap()`
- This function takes 2 arguements as `int*` which are stored in `a0` and `a1` respectively. From these arguments we can load their values to temporaries and ultimately perform the setting the value `a0` and `a1` with those from temporaries in reversed order

### `partition()`
- This function originally takes 3 arguements:
	- The base address of the given int array
	- left index
	- right index
- The return value of this function is to return the index of the pivot which is then used for subsequent recursive calls of the quicksort algorithm
- In this function's implementation, I used a lot of temporaries to hold not only values, but also addresses, and a lot of the times, you would see I used the following code snippet:
```c
t4 = t2 * 4;
t4 = a0 + t4;
t5 = *(int*)t4;
```
- `t4 = t2 * 4` (and `t2` holds the right index). This line means I want fetch the "real" index by multiplying the given index by 4 which is the size of an int. This matters because on the second line `t4 = a0 + t4`, I perform a pointer arithmetic. Only when the right offset is passed, can I then get the correct address to dereference on the third line and store it in the `t5` register

- Register liveness:
	- A register can be safely reused **if its old value is no longer needed**. This is the key compiler concept called **liveness analysis**.

### `qsort()`
- This function also orginally takes 3 arguments:
	- The base address of the given int array
	- left index
	- right index
- This function runs recursively on sub-arrays
	- The one containing elements < pivot value
	- The one containing elements > pivot value
- This function has is no special that the other previous twos

### Bugs that I encountered
1. `partition` clobbers the base pointer
- What does "clobber" mean?
	- To "clobber" a register means to overwrite its previous value and lose it
	- Inside `partition`, the register that was holding the array base address gets overwritten, and its original value is no longer available
- 