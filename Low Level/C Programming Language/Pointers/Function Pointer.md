## Definition
- a type of pointer that stores the address of a function, allowing functions to be passed as arguments and invoked dynamically

## Declaration
```
`return_type (*pointer_name)(parameter_types);`
```
- ***return_type:*** The type of the value that the function returns.
- **parameter_types:*** The types of the parameters the function takes.
- ***pointer_name:*** The name of the function pointer.

### Initialization
```c
ptr = &func // Using addressof operator  
ptr = func; // Without using addressof operator
```
⚠️ can also skip the address of operator as function name itself behaves like a constant function pointer

## Properties
1. Points to the memory address of a function in the code segment
2. Requires the exact function signature (return type and parameter list) ***(or we can type cast)***
3. Cannot perform arithmetic operations like increment or decrement
4. Supports array-like functionality for tables of function pointers

### Applications of  Function Pointers
1. As Argument (Callbacks)
```c
int add(int a, int b) { return a + b; }
void calc(int a, int b, int (*operation)(int, int)) {j printf("%d\n", op(a, b))}
int main(void) {
	calc(5, 10, add);
}
```

2. Emulate Member Function in Structure
```c
typedef struct {
	int w;
	int h;
	void (*set)(struct Rect)
	
} Rect;
```

3. Array of Function Pointers
```c
#include <stdio.h>

// Function declarations
int add(int a, int b) {
    return a + b;
}
int sub(int a, int b) {
    return a - b;
}
int mul(int a, int b) {
    return a * b;
}
int divd(int a, int b) {
    if(b!=0)
    return a / b;
    else
    return -1;
}

int main() {
    
    // Declare an array of function pointers
    int (*farr[])(int, int) = {add, sub, mul, divd};
    int x = 10, y = 5;

    // Dynamically call functions using the array
    printf("Sum: %d\n", farr[0](x, y)); 
    printf("Difference: %d\n", farr[1](x, y));
    printf("Product: %d\n", farr[2](x, y));
    printf("Divide: %d", farr[3](x, y));

    return 0;
}
```