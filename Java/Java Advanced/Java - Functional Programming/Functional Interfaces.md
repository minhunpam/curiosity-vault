## Definition:
- an interface that contains a single abstract method
- **SAM: single method rule**

```java
@FunctionalInterface  
interface Sprint {  
    public void sprint(int speed);  
}  
  
public class Tiger implements  Sprint{  
    @Override  
    public void sprint(int speed) {  
        System.out.println("Tiger sprints " + speed + " mph");  
    }  
  
    public static void  main(String[] args) {  
        Tiger tiger = new Tiger();  
        tiger.sprint(50);  
    }  
}
```
- ⚠️ Adding `FunctionalInterface` is optional 

If a functional interface includes an abstract method with the same signature as a public method found in `Object`, then those methods do not count toward the single abstract method test
```java
public inteface Dive {
	String toString();
	public boolean equals(Object o);
	public abstract int hashCode();
	public void dive(); // sing abstract method	
}
```


## Implementing Functional Interfaces with Lambdas
- The relationship between interfaces and lambda expressions is as follows: any functional interface can be implemented as a lambda expression
[[Java Lambda]]

## Working with Lambda Variables
### Parameter List
[[Java#`var` keyword|`var` keyword in lambda parameter list]]

### Local Variable inside the Lambda Body
```java
(a, b) -> { int c = 0; return 5; }
```

```java
public void variables(int a) {
	int b = 1;
	Predicate<Integer> p1 = a -> {
		int b = 0;
		int c = 0;
		return b == c;
	};
}
```
⚠️ In Java, lambda parameter names **cannot shadow** or reuse an existing variable name from the same scope
⚠️ lambdas share the same scope for local variables as the enclosing method


### Variables Referenced from the Lambda Body 
- lambda bodies are allowed to use `static` variables, instance variables, and local variables if they are `final` or effectively `final` -> Follow the same rules for access as [[Java Nested Classes (Inner Classes)#Local Class|Local]] and [[Java Nested Classes (Inner Classes)#Anonymous Class|Anonymous]] classes

## `Supplier`
- is used when you want to generate or supply values without taking any input
```java
@FunctionalInterface
public interface Supplier<T> {
	T get();
}
```

### Example
```java
Supplier<LocalDate> s1 = LocalDate::now;
Supplier<LocalDate> s2 = () -> LocalDate.now();

LocalDate d1 = s1.get();
LocalDate d2 = s2.get();

System.out.println(d1);
System.out.println(d2);
```

## `Consumer` and `Biconsumer`
- `Consumer` - when you want to do something with **a parameter** but not return anything
- `Biconsumer` - does the same thing that it takes 2 parameters
```java
@FunctionalInterface
public interface Consumer<T> {
	void accept(T t)
	// ommited default method
}
```

### Example with `Consumer`
```java
Consumer<String> c1 = System.out::println;
Consumer<String> c2 = x -> System.out.println(x);

c1.accept("Annie");
c2.accept("Annie");
```

```java
@FunctionalInterface
public interface BiConsumer<T, U> {
	void accept(T t, U u);
	// ommited default method
}
```

### Example with `Biconsumer`
```java
var map = new HashMap<String, Integer>();
BiConsumer<String, Integer> b1 = map::put;
BiConsumer<String, Integer> b2 = (t, u) -> map.put(t, u);

b1.accept("chicken", 7);
b2.accept("chick", 1);
```

## `Predicate` and `BiPredicate`
- `Predicate`- used when filtering and matching
- `BiPredicate` - just like `Predicate` except that it takes 2 parameters

```java
@FunctionalInteface
public interface Predicate<T> {
	boolean test(T t);
	// omitted default and static methods
}
```

```java
@FunctionalInteface
public interface BiPredicate<T, U> {
	boolean test(T t, U u);
	// omitted default and static methods
}
```

### Example of `Predicate`
```java
Predicate<String> p1 = String::isEmpty;
Predicate<String> p2 = x -> x.isEmpty();

System.out.println(p1.test(""));
System.out.println(p2.text(""));
```

### Example of `BiPredicate`
```java
BiPredicate<String> p1 = String::startsWith;
BiPredicate<String> p2 = (t, u) -> t.startsWaith(u);

System.out.println(b1.test("chicken", "chick"));
System.out.println(b2.test("chicken", "chick"));
```

## `Function` and `BiFunction`
- `Function` - turns one parameter into a value of a potentially different type and returning it
- `Bifunction` - turns two parameters into a value and returning it

```java
@FunctionalInterface
public interface Function<T, R> {
	R apply(T t);
	// ommited default and static methods
}
```

```java
@FunctionalInteface
public interface BiFunction<T, U, R> {
	R apply(T t, U u);
	// omitted default and static methods
}
```

### Example of `Function`
```java
Function<String, Integer> f1 = String::length;
Function<String, Integer> f2 = s -> s.length();

System.out.println(f1.apply("cluck")); // 5
System.out.println(f2.apply("cluck")); // 5
```

### Example of `BiFunction`
```java
BiFunction<String, String, String> b1 = String::concat;
BiFunction<String, String, String> b2 = (t, u) -> t.concat(u);

System.out.println(b1.apply("baby", "chick")); // baby chick
System.out.println(b2.apply("baby", "chick")); // baby chick
```








