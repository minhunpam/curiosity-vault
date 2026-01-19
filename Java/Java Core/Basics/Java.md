[[Setup]]
[[Java Basic Knowledge]]
[[Java - OOP]]
[[Java Errors]]
[[Java - File Handling]]
[[Java Data Structures]]
[[Spring Boot]]
[[Java Advanced]]

## `toString()` method - usage - how to override it properly #JavaMustKnow
### What does `toString()` do?
- returns a **textual representation of an object**
- by default, it returns a string like
```
ClassName@HexadecimalHashCode
```
which is not very human-readable

### How to override it properly
- Add annotation `@Override`
- In the method body, you can easily print the internal state of an object
```java
@Override
public String toString() {
    return "Person{name='" + name + "', age=" + age + "}";
}
```
### Advantages
1. For debugging and logging
2. For better readability in collections
3. For consistent representation

## `equals()` method #JavaMustKnow 
- used to compare equality of 2 Objects. The equality can be compared in 2 ways:
### Difference between `==` and `equals()`
- `equals()` - compares the 2 given strings based on the data/content of the string
- `==` - checks **reference equality** for objects and **value equality** for primitives
	- but before it can even compare, the compiler checks whether the 2 operands are type-compatible:
		- same type
		- inheritance relationship (parent <-> child)
		- one can be implicitly cast to the other
```java
public class Geeks {
    public static void main(String[] args) {
      
        String s1 = "HELLO"; 
        String s2 = "HELLO";
        String s3 =  new String("HELLO");

        System.out.println(s1 == s2);      // true
        System.out.println(s1 == s3);      // false
        System.out.println(s1.equals(s2)); // true
        System.out.println(s1.equals(s3)); // true
    }
}
```
- `s1 == s2 --> true`, because they have the same addresses in the [[Java String#String Constant Pool | String Constant Pool]]

### Shallow comparison:
- simply checks if 2 Object references - x and y refer to the same Object

### Deep comparison:
- a class provides its own implementation of `equals()` method in order to compare the Objects of that class with respect to state of the Objects == data members of Objects are to be compared with one another. 
* `equals()` is reflexive, symmetric and transitive


## `hashCode()` method #JavaMustKnow 
- returns the hashcode value as an Integer
- mostly used in hashing based collections like HashMap, HashSet, HashTable,...
- ***Must be overriden in every class which overrides `equals()` method***

#### General takeaways for `hashCode()`
- during an execution of the application, `hashCode()` returns the same Integer value (if invoked more than once on the same Object)
- not necessary same from one execution to another execution
- If 2 objects are `equals()` -> `hashCode()` must produce the same Integer
- If 2 objects are un-`equals()` -> not necessary the value is distinct but producing the distinct Integer value on each of the 2 Objects is better -> avoid collision for hashing-based data structures like `HashMap` and `HashTable`

## `.of(T value)` #Java9
- in `java.util.Optional`
- accepts value as parameter of type T -> create an Optional instance with this value
- Exception: throws `NullPointerException` if the specified value is null
```java
public static void main(String[] args)
    {
		try {
			// create a Optional
	        Optional<Integer> op1 = Optional.of(9455);
	        Optional<Integer> op2 = Optional.of(null);
	
	        // print value
	        System.out.println("Optional: " + op1); 
	        System.out.println("Optional: " + op2);
		}
		catch (Exception e) {
			System.out.println(e);
		}
    }

// Optional: Optional[9455]
// java.lang.NullPointerException
```

## Other uses of `.of()` that I saw
### `List.of(...)`
- is a static factory method -> create immutable list containing elements you pass to it.
```java
List<String> names = List.of("Larry", "Steve", "James", "Conan", "Ellen");
```

```java
names.add("NewName"); // Throws UnsupportedOperationException
```


## `var` keyword
- Type inference is used in `var` keyword in which it detects automatically the datatype of a variable base on the surrounding context
1. We can declare any datatype with the `var` keyword
2. `var` can be used in a local variable declaration
3. `var` cannot be used in an instance and global variable declaration
```java
class Demo {
	// instance variable
	var x = 50; // not allowed here
	public static void main(String[] args)
	{
		System.out.println(x);
	}
}
```
4. `var` cannot be used as a Generic type
```java
var<var> al = new ArrayList<>();
```
5. `var` cannot be used with the generic type
```java
var<Integer> al = new ArrayList<Integer>();
```
6. `var` cannot be used without explicit initialization
7. `var` cannot be used with Lambda Expression #FunctionalProgramming 
```java
import java.util.*;

@FunctionalInterface
interface myInt {
	int add(int a, int b);
}

class Demo7 {
	public static void main(String[] args) {
		// var cannot be used since they 
		// require explicit target type
		var obj = (a, b) -> a + b;
	}
}
```
- Even though `var` can be used in a lambda parameter list, if `var` is used for one of the types in the parameter list, then it must be used for all parameters in the list and each parameter type must be written out
6. `var` cannot be used for method parameters and return type



