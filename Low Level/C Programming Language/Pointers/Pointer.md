[[Function Pointer]]

>> Pointer is just a `size_t` number that is as big as the size of a register/ as big as 8 bytes

## Pointer Arithmetic
```c
int a[5] = {1,2,3,4,5};  
printf("%d\n", 2[a]);  
printf("%d\n", a[2]);  
printf("%d\n", *(a + 2));  
printf("%d\n", *(2 + a));
// These pointer arithmetic opertors are the same because of the associativity
```

