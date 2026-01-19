## Why should we use OOP principles?
- Resilience
- Effective communication
- Common "language"
## One very popular principle (collection) - SOLID
- **S**ingle-Responsibility
- **O**pen-Closed Principle
- **L**iskov-Substitution Principle
- **I**nteface Segregation Principle
- **D**ependency Inversion Principle

### **S**ingle Responsibility Principle (SRP)
1. Your class or method should have only one reason to change
2. Your class or method should have only one responsibility to take care of
```java
public class Invoice  
{  
	public void AddInvoice()  {
		// your logic 
	}  
	public void DeleteInvoice() {   
		// your logic  
	}  
	public void GenerateReport() {   
		// your logic  
	}  
	public void EmailReport() {   
		// your logic  
	}  
}
```
- Each method of this class may seem to adhere to SRP, but the `Invoice` class is taking care of multiple responsibilities

- **Why use it?**
	- **Improved Readability & Understandability**: Classes are smaller and focused on the task
	- **Easier Maintenance**: Changes related to one responsibility are isolated to a single class, reducing the risk of unrelated side effects
	- **Enhanced Testability**: Smaller classes with single responsibilities are easier to unit test in isolation
	- **Reduced Coupling**: Classes are less dependent on unrelated concerns
	- **Increased Reuseability**: A class focusing on 1 task is more likely to be reusable elsewhere

### **O**pen/Closed Principle (OCP)
- Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification
![[OOP2 - OCP.png]]
- The `Calculator` depends on the `Shape` abstraction. To add a new
shape (e.g., `Triangle`), create a new class inheriting from `Shape` and implement
`calculateArea()`. The Calculator does not need to be modified.

- **Statbility**: Reduces the risk of introducing bugs into existing, tested code when adding features
- **Maintainability**: Changes are localized to new code, not spread across existing modules
- **Flexibility**: Easier to adapt the system to new requirements
- **Reduced Testing Effort:** Only the new execution need rigorous testing; existing code (ideally) doesn't need full re-testing

### Liskov Substitution Principle (LSP)
- Subtypes must be substitutable for their base types without altering the correctness of the program.
- **This principle ensures that any class that is the child of a parent class should be usable in place of its parent without any unexpected behaviour**
- Focuses on the "is-a" relationship based on behavior, not just structure or inheritance syntax
![[OOP2 - LSP.png]]
- Violation: A Square "is-a" `Rectangle` geometrically, but behaviorally it violates the implied contract. A client expecting to set width and height independently on a `Rectangle` will break if given a `Square`. Square is not substitutable for `Rectangle` here
#### Why use it?
- **Reliable Polymorphism**: Ensures that using subclasses through base class references works as expected.
- **Correct Inheritance Hierarchies**: Helps model true **"is-a"** behavioral relationships.
- **Reduced Runtime Errors**: Avoids unexpected behavior when substituting subtypes.
- **Simplified Client Code**: Clients can trust that all subtypes behave according to the superclass contract, reducing the need for type checking `instanceof` or conditional logic.

### Interface Segragation Principle (ISP)
- Clients should not be forced to depend on methods they do not use
-  Prefer many small, client-specific interfaces over one large, general-purpose interface.
-  Classes should only need to implement methods that are relevant to them.
-  If a class implements an interface with methods it doesn’t need, it leads to empty implementations, exceptions, or unnecessary dependencies.
![[OOP2 - ISP.png]]
#### Why use it?
• **Reduced Coupling**: Clients only depend on the methods they actually call. Changes to unused methods in an interface won’t affect unrelated clients.
• **Improved Cohesion**: Interfaces are more focused and represent specific roles or capabilities.
• **Increased Flexibility & Reusability**: Smaller interfaces are easier to implement and combine.
• **Easier Implementation**: Classes don’t need to provide dummy implementations for irrelevant methods.
• **Better Design Clarity**: The roles and capabilities in the system are more explicit

## Dependency Inversion Principle (DIP)
1. High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces).
2. Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.
-  "Inversion" refers to inverting the traditional dependency direction **(High-level → Low-level)** to **(High-level → Abstraction ← Low-level)**.
-  Decouples policy (high-level logic) from implementation details (low-level specifics).
- Often implemented using Dependency Injection (DI) patterns.
![[OOP2 - DIP.png]]
- Problem: `NotificationService` is tightly coupled to the concrete `EmailSender`.
	- Changing to SMS requires modifying NotificationService.
- Solution: Both high-level and low-level depend on the abstraction.

#### Why use it?
- **Decoupling**: High-level modules are isolated from changes in low-level implementation details.
-  **Flexibility & Extensibility**: Easy to swap concrete implementations (e.g., change database, logging framework) without modifying high-level code.
-  **Testability**: Allows substituting real dependencies with mock objects or test doubles for unit testing high-level modules in isolation.
- **Reusability**: Both high-level and low-level modules (implementing abstractions) become more reusable.

## Composition over Inheritance (CoI)
- Favor composing objects (HAS-A relationship) over extending classes (IS-A relationship) to achiever polymorphic behavior and code reuse
-  Inheritance is a powerful tool, but can lead to rigid hierarchies and the "fragile base class" problem.
-  Composition allows for more flexible designs where behavior can be changed at runtime by composing with different objects.
-  Often used in conjunction with **Dependency Inversion** (depending on interfaces of composed objects).

#### Why use it?
- **Flexibility**: Behavior can be defined at runtime by composing with different objects, rather than being fixed at compile time by inheritance.
- **Avoids "Fragile Base Class" Problem**: Changes to a base class can unexpectedly break derived classes. Composition isolates components.
-  **Simpler Hierarchies**: Prevents deep and wide inheritance trees which can become hard to manage.
-  **Better Encapsulation**: Interactions happen through well-defined interfaces of composed objects, rather than relying on protected members of a base class.
-  **Testability**: Individual components can be tested in isolation more easily

## Patterns
### Singleton: Ensuring a single instance
- Guarantees only one instance of a class exists
- Provides a global point of access
- Useful for shared resources (e.g loggers, configs)
```java 
public class Logger {
	private static Logger instance;
	private Logger() {}
	public static Logger getInstance() {
		if (instance == null) {
			instance = new Logger();
		}
		return instance;
	}
	
	public void log(String msg) {
		System.out.println(msg);
	}
}
Logger.getInstance().log("Hello");
Logger.getInstance().log("Again :)");
```

### Observer: Keeping objects in sync
- Defines a notification calling system
- Useful for event handling, GUIs, and receive systems. 
```java
interface Observer {
	void update(String msg);
}

class Notifier {
	private List<Observer> observers = new ArrayList<>();
	void addObserver(Observer o) {
		observers.add(o);
	}
	void notifyAll(String msg) {
		for (Observer o : observers) {
			o.update(msg);
		}
	}
}

Notifier notifier = new Notifier();
notifier.addObserver(msg -> logger.info("Got: " + msg));
notifier.notifyAll("Hello Observers!");
```

### Builder Pattern with Lombok's @Builder
- The `@Builder` annotation from Lombok generates a builder for your class
- Simplifies object creation, especially for classes with many fields
- No need to write boilerplate builder code manually

```java
import lombok.Builder;
import lombok.ToString;
@Builder
@ToString
public class User {
	private String username;
	private int age;
	private String email;
}
User user = User.builder()
	.username("alice")
	.age(30)
	.email("alice@example.com")
	.build();
```

### Decorator: Adding behavior dynamically
- Attach additional responsibilities to an object dynamically
- More flexible than subclassing for extending functionality
- Useful for modifying behavior at runtime
```java
interface Entity { int getHealth(); }
class BaseEntity implements Entity {
	public int getHealth() { return 50; }
}

class Mage implements Entity {
	private Entity base;
	public Mage(Entity base) {
		this.base = base;
	}
	public int getHealth() {
		return base.getHealth() + 30;
	}
}

class Tank implements Entity {
	private Entity base;
	public Tank(Entity base) {
		this.base = base;
	}
	public int getHealth() {
		return base.getHealth() + 100;
	}
}

Entity mage = new Mage(new BaseEntity());
Entity mageTank = new Tank(new Mage(new BaseEntity()));
```

### Adapter: Bridging incompatible interfaces
- Allows objects with incompatible interfaces to work together
- Wraps an existing class with a new interface
- Useful when integrating with legacy code or third-party libraries

```java
interface PrintingService {
	void print(String doc);
}
class XeroxPrinter implements PrintingService {
	public void print(String doc) {
		System.out.println("Xerox prints: " + doc);
	}
}

interface Airprint {
	void airPrint(String doc);
}

class AirPrintAdapter implements PrintingService {
	privare AirPrint airPrinter;
	public AirPrintAdapter(AirPrint airPrinter) {
		this.airPrinter = airPrinter;
	}
	
	public void print(String doc) {
		airPrinter.airPrint(doc);
	}
}
```

### Factories
- Useful when you need to decide at runtime which subclass to instantiate
- Promotes loose coupling by hiding concrete classes
```java
interface Shape { double area(); }
class Rectangle implements Shape {
	private double w, h;
	public Rectangle(double w, double h) {
		this.v = w;
		this.h = h;
	}
	public double area() {
		return w * h;
	}
}

class Triangle implements Shape {
	private double side;
	public Triangle(double side) {
		this.side = side;
	}
	public double area() {
		return (Math.sqrt(3) / 4) * side * side;
	}
}

class ShapeFactory {
	public static Shape create(String type, double side) {
		switch(type) {
			case "square":
				return new Rectangle(side, side);
			case "circle":
				return new Triangle(side);
			case IllegalArgumentsException("Unknow type: " + type);
		}
	}
}

Shape s1 = ShapeFactory.create("square", 3);
Shape s2 = ShapeFactory.create("circle), 2);
```




