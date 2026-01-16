## Encapsulation
- "sensitive" data is hidden from users. To achieve this, you must:
	- declare class variables/ attributes as `private`
	- provide `public` getters & setters to access and update value of a `private` variable
```java
public class Person {
  private String name; // private = restricted access

  // Getter
  public String getName() {
    return name;
  }

  // Setter
  public void setName(String newName) {
    this.name = newName;
  }
}
```


## Inheritance
- subclass (child) - the class that inherits from another class
- superclass (parent) - the class being inherited from
- To inherit from a class, use the `extends` keyword
```java
class Vehicle {
  protected String brand = "Ford";        // Vehicle attribute
  public void honk() {                    // Vehicle method
    System.out.println("Tuut, tuut!");
  }
}

class Car extends Vehicle {
  private String modelName = "Mustang";    // Car attribute
  public static void main(String[] args) {

    // Create a myCar object
    Car myCar = new Car();

    // Call the honk() method (from the Vehicle class) on the myCar object
    myCar.honk();

    // Display the value of the brand attribute (from the Vehicle class) and the value of the modelName from the Car class
    System.out.println(myCar.brand + " " + myCar.modelName);
  }
}
```

## Calling parent's constructor inside child's constructor
[[Java - OOP#`super` keyword#Call Parent Constructor|Calling Parent Constructor]]

```java
public class BusinessException extends RuntimeException {
    public BusinessException(String message) {
        super(message);
    }
}
```

#### Why do we need `super(message)`
- Because `RuntimeException` already knows how to store and handle exception messages.  If you didn’t call `super(message)`, your `BusinessException` would not have the error message accessible by methods like `getMessage()`.

```java
try {
    throw new BusinessException("Title cannot be empty");
} catch (BusinessException e) {
    System.out.println(e.getMessage()); // prints "Title cannot be empty"
}
```

## Polymorphism
- many forms - it occurs when we have many classes that are related to each other by inheritance
```java
class Animal {
  public void animalSound() {
    System.out.println("The animal makes a sound");
  }
}

class Pig extends Animal {
  public void animalSound() {
    System.out.println("The pig says: wee wee");
  }
}

class Dog extends Animal {
  public void animalSound() {
    System.out.println("The dog says: bow wow");
  }
}
```
>>Similar to `virtual` in C++


## Abstraction
- is the process of hiding certain details and showing only essential information to the user
- Abstraction can be achieved with either **abstract classes** or **interfaces**
- [[Java Modifiers#Non-Access Modifiers | Non-Access Modifiers]]
- [[Java Interfaces]]

### Java Interfaces
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

### Abstract class
1. **If a class has at least one abstract method, the class must be abstract**
2. **Abstract class may have zero abstract methods**
	- Used to prevent instantiation
	- Act as a base type
3. **Abstract methods cannot have a body**
4. **Subclasses must implement all abstract methods**
