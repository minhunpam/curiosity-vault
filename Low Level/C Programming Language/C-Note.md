[[Pointer]]
[[C Macros]]
## `goto` Statement
- allows the program to jump to some part of the code, giving you more control over its execution
- useful in certain situations
	- error handling
	- exiting complex loops
> ***not recommended because it can make the code harder to read and maintain***

### Example of `goto`:
```C
#include <stdio.h>

int main() {
    int n = 0;  

    // If the number is zero, jump to
  	// jump_here label
    if (n == 0)
        goto jump_here;

    // This will be skipped
    printf("You entered: %d\n", n);

jump_here:
    printf("Exiting the program.\n");
    return 0;
}
```

## `void* memset(void* s, int c, size_t n)`
- copies the character `c` (an unsigned char) to the first `n` characters of array pointed to, by `s`
- This function is used to fill a contiguous block of memory with a specific value
⚠️ ***If `n` is larger than `s`'s size, it will be undefined***

### Initialize an integer array using `memset()`
```C
#include <stdio.h>
#include <string.h>

int main() {
	int arr[10];
	memset(arr, 0, sizeof(arr));
	// The arr will be filled 10 zeros
	return 0;
}
```

## `register` keyword
- faster than memory to access, so the variables which are most frequently used in a C program can be put in registers using the `register` keyword
- The keyword hints to the compiler that a given variable can be put in a register
	- It's the compiler's choice to put it in a register or not
	- Compilers themselves do optimizations and put the variable in a register
> There is no limit on the number of register variables in a C program, but the point is compiler may put some variables in the register and some not

⚠️ The location (address) of a variable declared with `register` cannot be accessed