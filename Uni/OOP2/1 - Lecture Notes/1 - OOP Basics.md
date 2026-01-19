Ideas behind Java
- Platform independence: write once, run everywhere
- Wide applicability: from embedded to desktop, server and compute clusters
- Rich set of class libraries

## The Java Stack: From Source to Bytecode
- Java source (`.java`) is compiled by javac into a `.class`
- Bytecode is execute  by the JVM on any supported platform
## Java JDK vs JRE
1. **JDK (Java Development Kit)**:
	- Full-featured software development kit for Java
	- Includes the JRE, compiler (javac), debugger, and development tools
2. **JRE (Java Runtime Environment)**:
	- Provides libraries, Java Virtual Machine (JVM), and other components to run Java applications
	- Does not include developement tools (compiler)
- **Summary: JDK = JRE + Development tools**

## OOP recaps
- **Classes and Objects**:
	1. Class: Blueprint for creating objects
	2. Object: Instance of a class created using the `new` keyword
- **Inheritance**:
	- Allows a class (subclass) to inherit methods and properties from another class (superclass)
	- Use the `extends` keyword
	- IS-A relationship
- **Encapsulation**:
	- Protects object data by using private fields and public getters/ setters
- **Polymorphism**:
	- Ability of a method to perform different tasks based on the object (**Overriding**)
	- C++: `virtual` keyword
- **Abstraction**:
	- Hides the implementation details from the user
	- Achieved through abstract classes and/or interfaces
	- C++: pure virtual methods

## Interfaces in Java
- An interface defines a contract of methods that implementing classes must provide
- Interfaces contain method signatures (No implementation)
- A class uses the `inplements` keyword to adopt an interface
- CAN-DO relationship

## Nested classes in Java
- Java allows classes to be defined within other classes (nested classes)
- Types:
	1. **static nested classes**
		- Cannot access `Outer.this` or outer fields directly
	2. **inner class (non-static)**
		- Can access outer class data using `Outer.this`
		```java
		class Outer {
			private int data = 42;
			class Inner {
				void display() {
					System.out.println("Inner class, data = " + Outer.this.data");
				}
			}
		}
		```
- Useful for logicall grouping classes and increasing encapsulation

## Access modifiers in Java

| Modifier                     | Class | Package | Subclass | World |
| ---------------------------- | ----- | ------- | -------- | ----- |
| public                       | 1     | 1       | 1        | 1     |
| protected                    | 1     | 1       | 1        | 0     |
| no modifier/ package-private | 1     | 1       | 0        | 0     |
| private                      | 1     | 0       | 0        | 0     |
## Java vs. C++ differences
- Memory management: Java has automatic garbage collection, C++ requires manual management
- Inheritance: Java supports single inheritance, C++ supports multiple inheritance
- Pointers: Java does not use explicit pointers
- Platform Independence: Java programs run on the JVM (which has support for a wide range of platforms - from printers to ARM servers)

## Interfaces: Solution to multiple inheritance
- Java does ot support multiple inheritance with classes
- Interfaces allow a class to implement multple types

## Exception Handling in Java
- Use `try`,`catch`,`finally` blocks
- In Java, the `throws` keyword is used in method signatures to declare that a method may throw certain checked exceptions
- This informs callers that they must handle or further declare these exceptions
- The handling checked at compile time!

## Java generics
- Provide type safety for collections and classes - only for types
```java
class Box<T> {
	private T value;
	public void set(T value) { this.value = value; }
	public T get() { return value; }
}
Box<Integer> intBox = new Box<>();
intBox.set(123);0
```

## Java collections overview
- Generics power the collections in Java (similar to the `std::list`)....
- Main interfaces: `List`, `Set`, `Map`
1. **List: ordered, allows duplicates**
2. **Set: unordered, no duplicates**
3. **Map: key-value pairs**

## Reflection: Listing methods of a class
- Java reflection can be used to inspect methods of a class at runtime
- Example: print all method names of a class

```java
class Parent {
	protected boo() {}
}
class Example {
	public void foo() {}
	private int bar(int x) { return x; }
}

for (Method m : Example.class.getDeclaredMethdods()) {
	logger.info(m.getName());
}
// Output: foo, bar
```

## Annotations in Java
- Annotations provide metadata about code to the compiler or runtime
- Common built-in annotations: `@Override`, `@Deprecated`, `@SuppressWarnings`
- Custom annotations can be defined for frameworks and tools

### Defining custom annotations
- Define with `@interface` keyword
- Can specify elements (like parameters)
```java
@interface MyAnnotation {
	String value();
}

@MyAnnotation("example")
class Demo() {}
```

### Reflection: Using `.getAnnotation` and custom annotations
- Java reflection allows inspecting classes, fields, and annotations at runtime
- Example: mark fields for database storage using a custom `@DBField` annotation
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
@interface DBField {}

class User {
	@DBField
	String username;
	int age;
}


for (Field field : User.class.getDeclaredFields()) {
	if (field.getAnnoation(DBField.class) != null) {
		logger.warn("Add to DB: " + field.getName());
	}
}
```

