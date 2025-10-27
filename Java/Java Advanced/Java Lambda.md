- a short block of code which takes in parameters and returns a value
- similar to methods, but they do not need a name
- can be implemented right in the body of a method
## Syntax
### Short form
```java
a -> a.canHop()
// a : parameter name
// -> : arrow
// a.canHop() : body
```

### Long form
```java
(Animal a) -> { return a.canHop(); }
// Animal: type
// a : parameter name
// -> : arrow
// { return a.canHop(); } : body

```
⚠️ Parentheses can be omitted only if there is a single parameter and its type is not explicitly stated
⚠️ Java doesn't require you to type `return` or use a semicolon when no braces are used
## Lambda Expressions rely on the notion of **Deferred Execution**
- _Deferred Execution_ = code is specified now but runs later. 

## Writing Lambda Expressions:
- The left side of Lambda Expression lists the variables
	- must be compatible with the type and number of input parameters of the functional interface's single abstract method
- The right side of Lambda Expression represents the body of the expression
	- must be compatible with the return type of the functional interface's abstract method
