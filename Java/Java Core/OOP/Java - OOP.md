- Class - template for objects
- Object - instance of a class
- Create an object
	- to create an object of a class, specify the class name, followed by the object name and use the keyword `new`
```java
public class Main {
  int x = 5;

  public static void main(String[] args) {
    Main myObj = new Main();
    System.out.println(myObj.x);
  }
}
```

## Using Multiple Classes
- You can also create an object of a class and access it in another class. This is often used for better organization of classes (one class has all the attributes and methods, while the other class holds the `main()` method (code to be executed)).
```java
// Main.java
public class Main {
  int x = 5;
}

// Second.java
class Second {
  public static void main(String[] args) {
    Main myObj = new Main();
    System.out.println(myObj.x);
  }
}
```

## `static` vs. `public`
- `static` - can be accessed without creating an object of the class
- `public` - can only be accessed by objects

## Java Constructors
- constructor is a special method that is used to initialize objects
- the constructor is called when an object of a class is created
- constructor can also take parameters -> used to initialize attributes

## `this`
- refers to the current object in a method or constructor
- is often used to avoid confusion when class attributes have the same name as method or constructor parameters
### Calling a constructor from another constructor
- can also use `this()` to call another constructor in the same class -> useful when you want to provide default values or reuse initialization code instead of repeating it
* ⚠️ `this()`  must be the **first statement** inside the constructor
```java
public class Main {
  int modelYear;
  String modelName;

  // Constructor with one parameter
  public Main(String modelName) {
    // Call the two-parameter constructor to reuse code and set a default year    
    this(2020, modelName);
  }

  // Constructor with two parameters
  public Main(int modelYear, String modelName) {
    // Use 'this' to assign values to the class variables
    this.modelYear = modelYear;
    this.modelName = modelName;
  }

  // Method to print car information
  public void printInfo() {
    System.out.println(modelYear + " " + modelName);
  }

  public static void main(String[] args) {
    // Create a car with only model name (uses default year)
    Main car1 = new Main("Corvette");

    // Create a car with both model year and name
    Main car2 = new Main(1969, "Mustang");

    car1.printInfo();
    car2.printInfo();
  }
}
```

[[Java Modifiers]]
[[OOP Properties]]

## `super` keyword
- `super` keyword is used to refer to the **parent class** of a subclass
- The most common use of the `super` keyword is to eliminate the confusion between superclasses and subclasses that have methods with the same name
- can be used in 2 main ways:
	- to access attributes and methods from the parent class
	- to call the parent class constructor
### Access Parent Methods
```java
class Animal {
  public void animalSound() {
    System.out.println("The animal makes a sound");
  }
}

class Dog extends Animal {
  public void animalSound() {
    super.animalSound(); // Call the parent method
    System.out.println("The dog says: bow wow");
  }
}
```

### Access Parent Attributes
```java
class Animal {
  String type = "Animal";
}

class Dog extends Animal {
  String type = "Dog";

  public void printType() {
    System.out.println(super.type); // Access parent attribute
  }
}
```

### Call Parent Constructor using `super(...)`
- Every class in Java can call its **parent class constructor** using the keyword `super(...)`

```java
class Animal {
  Animal() {
    System.out.println("Animal is created");
  }
}

class Dog extends Animal {
  Dog() {
    super(); // Call parent constructor
    System.out.println("Dog is created");
  }
}
```
- ⚠️ ***The call to `super()` must be the first statement in the subclass constructor***

#### A typical example when creating custom Exception
```java
public class NotFoundException extends RuntimeException {
	public NotFoundException(String message) {
		super(message);
	}
}

```



## Enums (short for "enumeration" - "specifically listed")
- special "class" that represents a group of constants
- constants should be uppercase
```java
enum Level {
  LOW,
  MEDIUM,
  HIGH
}
```

- enums are often used in `switch` statements to check for corresponding values

### Loop through an Enum
- The enum type has a `value()` method, which returns an array of all enum constants
- Useful when want to loop through the constants of an enum
```java
for (Level myVar : Level.values()) {
  System.out.println(myVar);
}
```

⚠️ `enum` can have attributes and methods
- enum constants are `public`, `static`, `final` (unchangeable - cannot be overriden)
- cannot be used to create objects AND cannot extend other classes (but can implement interfaces)
```java
public interface Operation {
    double apply(double x, double y);
}

public enum BasicOperation implements Operation {
    ADD {
        public double apply(double x, double y) {
            return x + y;
        }
    },
    SUBTRACT {
        public double apply(double x, double y) {
            return x - y;
        }
    },
    MULTIPLY {
        public double apply(double x, double y) {
            return x * y;
        }
    },
    DIVIDE {
        public double apply(double x, double y) {
            return x / y;
        }
    };
}
```

### Where should an enum be placed #JavaGoodToKnow 
1. Own file (top-level enum)
```java
public enum Season {
	SPRING, SUMMER, FALL, WINTER
}
```

2. Same file as another top-level type
```java
enum Season {
	SPRING, SUMMER, FALL, WINTER
}
public class Main { 
	//... 
}
```
- Java allows multiple top-level types per file, but only one can be `public`, and the file name must match that public type
3. Nested inside a class (member enum)
```java
public class Main {
	public enum Seasons { 
		//... 
	}
}
```
- Great when the enum is only meaningful for that class

### Enums can have more in them than just a list of values
```java
public enum Season {
	WINTER("low"), SPRING("medium"), SUMMER("high"), FALL("medium");
	private final String expectedVisitors;
	// modifier private is optional -> because enum constructor is implicitly 
	// marked private
	private Seasons(String expectedVisitors) {
		this.expectedVisitors = expectedVisitors;
	}

	public void printExpectedVisitors() {
		System.out.println(expectedVisitors);
	}
}
```
- `final` -> immutability -> good practice, because enum values are shared by all processes, it would be problematic if one of them could change the value inside the enum
- The fact that enum ctor is private, is reasonable, because you can't extend an enum and the constructor can be only be called within the enum itself.
	- the `public` or `protected` enum ctor will not compile

⚠️ ***compiler requires that the list of values always be declared first***

### `valueOf()`
