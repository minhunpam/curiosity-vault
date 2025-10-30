- is declared outside any function
- global variables do not stay limited to a specific function == one can use any given function to access and modify the global variables

## Advantages and Disadvantages of Global Variable
| Advantages                                                  | Disadvantages                                                                                                |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| can be accessed by all the functions present in the program | The value of a global variable can be changed accidentally                                                   |
| Only a one-time declaration                                 | If we use a large number of global variables, then there is a high chance of error generation in the program |
| all the functions are accessing the same data               |                                                                                                              |

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
