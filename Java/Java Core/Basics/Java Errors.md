
Types of errors in Java:
- compile-time error:
- runtime error
- logical error
## Common Compile-time errors
1. Missing semicolon
2. Undeclared variables 
3. Mismatched types

## Common Runtime Errors
1. Division by Zero
2. Array index out of bounds

## Java Exceptions
- When an error occurs, Java will normally stop and generate an error message = Java will throw an **exception** (throw an error).
### Exception handling
- lets you catch and handle errors during runtime - so your program doesn't crash
- `try` - allows you to define a block of code to be tested for errors while it is being executed
- `catch` allows you to define a block of code to be executed, if an error occurs in the try block
- `try` and `catch` come in pair
```java
try {
  //  Block of code to try
}
catch(Exception e) {
  //  Block of code to handle errors
}
```

### Finally
- `finally` lets you execute code, after `try...catch`, regardless of the result

### The throw keyword
- `throw` statement allows you to create a custom error
- `throw` is used together with an **exception type**

### Distinguishing between `throws` and `throws`
- The `throw` keyword means an exception is actually being thrown
- The `throws` keyword **indicates that the method merely has the potential to throw that exception**
#### Example:
```java :n
public String getDataFromDatabase() throws SQLException {
	throw new UnsupportedOperationException();
}
```
- `throws SQLException` - the method might or might not throw `SQLException`
	- Since this is a checked exception, the caller needs to handle or declare it
- `throw new UnsupportedOperationException()` - actually does throw an `UnsupportedOperationException`



