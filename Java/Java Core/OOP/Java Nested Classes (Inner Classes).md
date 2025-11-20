- The purpose of nested classes is to group classes that belong together, which makes your code more and readable and maintainable
```java
class OuterClass {
  int x = 10;

  class InnerClass {
    int y = 5;
  }
}

public class Main {
  public static void main(String[] args) {
    OuterClass myOuter = new OuterClass();
    OuterClass.InnerClass myInner = myOuter.new InnerClass();
    System.out.println(myInner.y + myOuter.x);
  }
}
```

## Non-static Inner Class
- Unlike "regular" class, an inner class can be `private` or `protected`. If you don't want out objects to access the inner class
* Inner class have the following properties:
	1. can be declared `public`, `protected`, `private`
	2. can extend any class and implement interfaces
	3. can be marked `abstract` or `final`
	4. cannot declare `static` fields/ methods, except for `static final` fields
	5. can access members of the outer class including `private` members

## Static Inner Class
- An inner class can also be `static`, which means you can access it without creating an object of the outer class
- The enclosing class can refer to the fields and methods of the `static` nested class
```java
class OuterClass {
  int x = 10;

  static class InnerClass {
    int y = 5;
  }
}

public class Main {
  public static void main(String[] args) {
    OuterClass.InnerClass myInner = new OuterClass.InnerClass();
    System.out.println(myInner.y);
  }
}

// Outputs 5
```
⚠️ ***a `static` inner class does not have access to members of the outer class*** ⚠️
- Trade-off for `static` inner class:
	- can't access instance variables/ methods in the outer class directly (can be done by giving explicitly a reference to an outer class/ instantiating the outer class)
	- can access the static members of the outer class directly

## Local Class
- a nested class defined within a method. 
- a local class declaration doesn't exist until the method is invoked, and it goes out of scope when the method returns <-> you can create instance only from within the method. Those instances can still be returned from the method

```java
public class PrintNumbers {
	private int length = 5;
	public void calculate() {
		final int width = 20;
		class MyLocalclass {
			public void multiply() {
				System.out.println(length * width);
			}
		}
		MyLocalClass local = new MyLocalClass();
		local.multiply();
	}

	public static void main(String[] args) {
		PrintNumbers outer = new PrintNumber();
		outer.calculate();
	}
}
```
### Properties of local class
1. doesn't have access modifier
2. cannot be declared static and can't declare static fields/ methods (except `static final`)
3. have access to all fields and methods of the enclosing class (when defined in an instance method)
4. can access local variables if the variables are `final` or effectively `final`

#### What is effectively `final`?
- a variable is **effectively final** if **its value is not changed after it is assigned**, even though it is **not explicitly declared with the `final` keyword**


### Why can local class only access local variables if they are `final` or effectively `final`
```java
void doSomething() {
    int x = 10;
    class Local {
        void printX() {
            System.out.println(x);
        }
    }
}
```
- when the method runs, a `.local` file is created, but the method's local variables live on the stack and disappear when the method returns.
-> Solution: instead of pointing to the original stack variable, the compiler copies the value of local variables into `.class` file
- Because a copy is made at the moment the local class object is created. If the original local variable could change later, the local class's would be out of sync with the method's variable -> unexpected, inconsistent behavior

## Anonymous Class
- a specialized form of a local class that does not have a name
- is declared and instantiated all in one statement using the `new` keyword
- anonymous classes are required to 
	- ***Extend an existing class*** or 
	- ***Implement an interface***
### Extend an existing class
```java
public class ZooGiftShop {
	abstract class SaleTodayOnly {
		abstract int dollarsOff();
	}

	public int admission(int basePrice) {
		SalesTodayOnly sale = new SaleTodayOnly() {
			int dollarsOff() { 
				return 3;
			}
		};
		return basePrice - sale.dollarsOff();
	}
}
```

### Implement an interface
```java
public class ZooGiftShop {
	interface SaleTodayOnly {
		int dollarsOff();
	}

	public int admission(int basePrice) {
		SalesTodayOnly sale = new SaleTodayOnly() {
			public int dollarsOff() { 
				return 3;
			}
		};
		return basePrice - sale.dollarsOff();
	}
}
```


### Others
- You can anonymous class right where they are needed, even if that is an argument to another method
- You can also define anonymous classes outside a method body
```java
public class Gorilla {
	interface Climb() {}
	Climb climbing = new Climb() {}
}
```
- this may look weird, but the `{}` after the interface name indicates that this is an anonymous inner class implementing an interface

