## Definition of Macros
- a symbolic name or constant that represents a value, expression, or code snippet
- are defined using the `#define`directive, and when encountered, the preprocessor substitutes it with its defined content
### Types of Macros
1. Object-like Macros
2. Chain Macros
```c
#define INSTAGRAM FOLLOWERS
#define FOLLOWERS 138
```
3. Multi-Line Macros
- remember to use **backslash (`\`)**
```c
#define ELE 1, \
			2, \
			3
```
4. Function-like Macros
```c
#define min(a, b) (((a) < (b)) ? (a) : (b))
```


## `#define` in C
- a **preprocessor directive** that is used to define macros
- macros are the identifiers defined by `#define` which are replaced by their value before compilation
- can define constants and functions like macros using `#define`

### Syntax:
1. For defining Constants
```c
#define MACRO_NAME value
```
2. For defining Expressions
```c
#define MACRO_NAME (expression within brackets)
```
3. For defining Expressions with parameters
```c
#define MACRO_NAME(ARG1, ARG2,...) (expressions within brackets)
```


