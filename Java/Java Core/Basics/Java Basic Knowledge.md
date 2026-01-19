- Class name should always start with an uppercase first letter (`Main`)
- `System` is a built-in Java class that contains useful members, such as `out` , `println()`
- `print()` - similar to `println()`
	- only difference: does not insert a new line at the end of the output
- can perform mathematical calculations inside the `println()` (`System.out.println(3 + 3)`)

#### Java Comments
- Single-line comments: `//`
- Multi-line Comment:
```
/* multi
* line
* comments
* */
```

#### Java Variables
- Containers for storing data values 
- Syntax: `type variableName = value;`
- can declare a variable without assigning the value, and assign the value later
- if assign a new value to an existing var, it will overwrite the previous value
- if you don't want others to overwrite existing value, use the `final` keyword -> declare the variable as "constant" -> unchangeable/ read-only
```
final int myNum = 15;
myNum = 20; // will generate an error: cannot assign a value to a final variable
```
- to declare more than one variable of the ***same type***, you can use a comma-separated list
```
int x = 5, y = 6, z = 7;
```
- can also the ***same value*** to multiple variables in one line:
```
int x, y, z;
x = y = z = 50;
```

- all Java variables must be identified with unique names which are called **identifiers**


#### Java Data Types
- Primitive data types: `byte`, `long`, `int`, `float`, `double`, `boolean`, `char`
	- ⚠️`float` or `double` --> safter to use `double` for most calculations as it has better precision of floating point value
	- Scientific number: 
		- "e" to indicate the power of 10 ("e" could be lowercase or uppercase)
		`float f1 = 35e3f`
		`double d1 = 12E4d`
- 
- Non-primitive data types: `String`, Arrays and Classes
	- are created by the programmer (except for `String`)
	- can be used to call methods to perform certain operations, whereas primitive types cannot
	- primitive types start with a lowercase letter (like `int`), while non-primitive typically starts with an uppercase letter (like `String`)
	- primitive types always hold a value, whereas non-primitive types can be `null`

#### Java Type Casting
- There are 2 types of casting:
	- **Widening Casting (automatically)** - convert a smaller type to a larger type size
		`byte` -> `short` -> `char` -> `int` -> `long` -> `float` -> `double`
	- **Narrowing Casting (manually)** - convert a larger type to a smaller type size
		`double` -> `float` -> `long` -> `int` -> `char` -> `short` -> `byte`
- Widening Casting:
```
int myInt = 9;
double myDouble = myInt; // Automatic casting: int to double
```

* Narrowing Casting:
```
double myDouble = 9.78d;
int myInt = (int) myDouble; // Manual casting by placing the type in parentheses `()` in front of the value
```

#### Java Strings
- used for storing texts
- String length: `myString.length()`
- String methods:
	- `.toUpperCase()` , `.toLowerCase()`
	- `indexOf()` returns the index (the position) of the first occurrence of a specified text in a string(including whitespace)
```java
String txt = "Please locate where 'locate' occurs!";
System.out.println(txt.indexOf("locate")); // Outputs 7
```
* String Concatenation:
	* `+` operator can be used to concatenate 2 or more strings
	* can also use `concat()` method to do the same thing
```java
String fname = "Hung ";
String lname = "Pham";
System.out.println(fname.concat(lname));
```
- ⚠️ If you add a number and a string, the result will be a string concatenation
```java
String x = "10";
int y = 20;
String z = x + y; // z will be 1020 (a String)
```

#### Java Math
- `Math.max(x, y);`
- `Math.min(x, y);`
- `Math.sqrt(x);`
- `Math.abs(x);` - absolute value of x
- `Math.random();` - random number between [0.0, 1.0)

#### Java for-each loop
- "for-each" loop -> used exclusively to loop through elements in an array (or other data structures)
```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
for (String i : cars) {
	System.out.println(i);
}
```

### Java break and continue
- `break` - jump out of a loop
- `continue` - breaks one iteration (in the loop), if a specified condition occurs, and continues with the next iteration in the loop.

#### Java Multi-dimensional Array
- The inner arrays of a 2-dimensional array do not have to have the same size a.k.a "jagged arrays"/ "ragged arrays"
```java
int[][] jagged = {
    {1, 2},
    {3, 4, 5, 6},
    {7}
};
```

#### Java method
- **method**/function is a block of code which only runs when it is called
- can pass data (parameters) into a method
- why? - Define the code once, and use it many times
- parameters - local variable of a method, written in the function signature
	- Add as many as you want, comma-separated 
- arguments - values passed in the method
	- when passed in, same amount and same order
##### Method overloading
- multiple methods can have the same name with different parameters as long as the number and/or type of parameters are different
```java
int myMethod(int x)
float myMethod(float x)
double myMethod(double x, double y)
```

##### Java Scope
- variables are only accessible inside the region they are created <- scope
- ***Method Scope***
	- variables declared directly inside a method are available anywhere in the method
- ***Block code***
	- a block -> all of the code between curly braces `{}`
	- variables declared inside blocks of code are only accessible by the code between curly braces

## Java Date
- import package `java.time` package to work with the date and time API
- the package includes the following classes:
	- `localDate` - represents a date (year, month, day)
	- `localTime` - represents a time (hour, minute, second, nanoseconds)
	- `localDateTime` - represents both a date and a time
	- `DateTimeFormatter` - formatter for displaying and parsing date-time objects
- Display current date, time, datetime with `now()` method

###  Formatting Date and Time
- use `ofPattern()` to specify the pattern
- use `format()` to format the date and time
```java
import java.time.LocalDateTime; // Import the LocalDateTime class
import java.time.format.DateTimeFormatter; // Import the DateTimeFormatter class

public class Main {
  public static void main(String[] args) {
    LocalDateTime myDateObj = LocalDateTime.now();
    System.out.println("Before formatting: " + myDateObj);
    DateTimeFormatter myFormatObj = DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm:ss");

    String formattedDate = myDateObj.format(myFormatObj);
    System.out.println("After formatting: " + formattedDate);
  }
}
```

## Varargs
- provide a short-hand for methods that support an arbitrary number of parameters of one type
- using an array under the hood
```java
public String formatWithVarArgs(String... values) { // .... }
```

### Note:
- each method can ONLY have one varargs parameter
- the varargs argument must be the last parameter

### Heap Pollution

[[Java String]]
