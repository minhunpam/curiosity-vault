- An `interface` is a completely "abstract class" that is used to group related methods with empty bodies
```java
// interface
interface Animal {
  public void animalSound(); // interface method (does not have a body)
  public void run(); // interface method (does not have a body)
}
```
- To access the interface methods, the interface must be "implemented" by another class with the `implements` keyword
```java
// Interface
interface Animal {
  public void animalSound(); // interface method (does not have a body)
  public void sleep(); // interface method (does not have a body)
}

// Pig "implements" the Animal interface
class Pig implements Animal {
  public void animalSound() {
    // The body of animalSound() is provided here
    System.out.println("The pig says: wee wee");
  }
  public void sleep() {
    // The body of sleep() is provided here
    System.out.println("Zzz");
  }
}
```

> Notes on Interface:
> - interface cannot be used to create objects
> - interface methods do not have a body
> - on implementation of an interface, you must override all of its methods
> - interface methods are by default `abstract` and `public`
> - interface attributes are by default `public`, `abstract` and `final`
> - an interface cannot contain a constructor (as it cannot be used to create objects)

#### Why and when interfaces?
1. achieve security - hide certain details and only show the important details of an object (interface).
2. Java does not support **multiple inheritance** (a class can only inherit from one superclass)
	- a class can implement multiple interfaces

```java
interface FirstInterface {
  public void myMethod(); // interface method
}

interface SecondInterface {
  public void myOtherMethod(); // interface method
}

class DemoClass implements FirstInterface, SecondInterface {
  public void myMethod() {
    System.out.println("Some text..");
  }
  public void myOtherMethod() {
    System.out.println("Some other text...");
  }
}
```

## `default` interface method #Java8 
- A `default` method - a method: `default` keyword + method body
### Motivation:
- comes from the concept that it is viewed as an abstract interface method with a default implementation
- the class has the option of overriding the default method, but if it does not, then the default implementation will be used

## Purpose
- **Backward Compatibility**: allows you to add a new method to an existing interface, without the need to modify older code that implements the interface

### Default interface method definition rules
1. declared within an interface
2. must be marked with the default + method body
3. ***assumed to be `public`***
4. cannot be marked `abstract` `final` `static`
5. may be overridden by a class that implements the interface
6. if a class overrides 2 or more `default` methods with the same method signature -> the class must override the method

## `static` interface method #Java8 
## Static Interface Method Definition Rules
1. marked with `static` keyword + include a method body
2. ***assumed to be `public`***
3. cannot be marked `abstract` or `final`
4. cannot be inherited AND cannot be accessed in a class implementing the interface without a reference to the interface name
	-> This solves the the **multiple inheritance problem** of `static` interface methods by not allowing them to be inherited
	-> applies for sub-interfaces and classes that implement the interface

```java
interface WalkBunny {  
    static int getSpeed() {  
        return 5;  
    }  
}  
  
interface RunBunny {  
    static int getSpeed() {  
        return 10;  
    }  
}  
  
public class Bunny implements WalkBunny, RunBunny {  
    public void printDetails() {  
        System.out.println(WalkBunny.getSpeed());  
        System.out.println(RunBunny.getSpeed());  
    }  
  
    public static void main(String[] args) {  
        Bunny bunny = new Bunny();  
        bunny.printDetails();  
    }  
}
```

### `private` Interface methods #Java9 
#### Why?
- can be used to reduce code duplication
```java
interface ISchedule {  
    default void wakeUp() { checkTime(7);}  
    default void haveBreakfast() { checkTime(9); }  
    default void haveLunch() { checkTime(12); }  
    default void workOut() { checkTime(18); }  
  
    private void checkTime(int hour) {  
        if (hour > 17) System.out.println("You're late!");  
        else System.out.println("You have " +(17-hour)+" hours left");  
    }  
}
```

#### `private` Interface Method Rules
1. must be marked `private` and include a method body
2. may be called only by `default` and `private` (non-static) methods within the interface definition

### Private Static Interfaces Method Definition Rules
1. must be marked with `private` and `static` include a method body
2. maybe called only by other methods within the interface definition

## Why can't a private method be called from a private static method? #JavaMustKnow 
- a `private` non-static method belongs to an _instance_
- a `private static` method belongs to the _class/interface itself_
	- has **no instance context** (`this`) -> cannot directly call a non-static method

## Functional Interfaces
[[Functional Interfaces]]

## Can a function/ method return an interface?
- What this means: "I don’t care _how_ this object is implemented —  I only promise it supports this behavior."
- This is powerful because the caller does not depend on concrete classes

## Java Rules
- In a Java Interface, any method without a modifier is implicitly
```java
public abstract
```


