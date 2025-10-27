[[Pointer]]
[[C Macros]]

## `extern` keyword
- used to declare a variable or a function whose definition is present in some other file
- In the file, where we want to use the variable/ function, we have to provide `extern` declaration
### Access global variables across multiple files
- First create a file that contains the definition of a variable. It must be global variable

```c
//src.c
int ext_var = 22;
```

```c
//main.c
extern int ext_var;

int main() {
	printf("%d", ext_var);
	return 0;
}

// Output: 22
```

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

