- wrapper classes provide a way to use primitive data types as objects
	- `byte` -> `Byte
	- `short` -> `Short`
	- `int` -> `Integer`
	- etc.
- sometimes your must use wrapper classes, for example when working with Collection objects, such as `ArrayList`, where primitive types cannot be used
```java
ArrayList<Integer> myNumbers = new ArrayList<Integer>();
```

## Creating Wrapper objects
- to create a wrapper object, use the wrapper class instead of the primitive type

## Useful methods
- Since you're working with objects, you can use certain methods to get information about the specific object. For example, the following methods are used to get the value associated with the corresponding wrapper object: `intValue()`, `byteValue()`, `shortValue()`,...
- `toString()` -> convert wrapper object to strings

## Autoboxing and Unboxing
- **Autoboxing** - primitive -> corresponding wrapper
```java
Integer pounds = 120;
```
- Unboxing - wrapper class -> primitive
```java
//Autoboxing
Character letter = "robot".charAt(0);
// Unboxing
char r = letter;
```

