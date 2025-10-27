- special notes added to the Java code, starting with `@` symbol
- don't change how the program runs, but give  **extra information** to the compiler/ tools

## Built-in Annotations
- `@Override` - indicates that a method overrides a method in a superclass
```java
class Animal {  
  void makeSound() {  
    System.out.println("Animal sound");  
  }  
}  
  
class Dog extends Animal {  
  @Override  
  void makeSound() {  
    System.out.println("Woof!");  
  }  
}
```

- `@Deprecated` - marks a method/ class as outdated or discourages from use
	- warns developers not to use a method because it may be removed or replaced in the future

```java
public class Main {  
  @Deprecated  
  static void oldMethod() {  
    System.out.println("This method is outdated.");  
  }  
  
  public static void main(String[] args) {  
    oldMethod(); // This will show a warning in most IDEs  
  }  
}
```
- `@SuppressWarnings` - tells the compiler to ignore certain warnings
	- useful when working with old code/ when you're sure the operation is safe
```java
import java.util.ArrayList;  
  
public class Main {  
  @SuppressWarnings("unchecked")  
  public static void main(String[] args) {
	// using raw types like ArrayList without specifying a type ArrayList<String> usually causes an "unchecked" warning 
    ArrayList cars = new ArrayList();  
    cars.add("Volvo");  
    System.out.println(cars);  
  }  
}
```

## `@SpringBootApplication` 
- is a combination of 3 important annotations:
	1. [[IOC Container#`@Configuration`|`@Configuration`]]
	2. `@EnableAutoConfiguration` 
	3. [[IOC Container#`@ComponentScan`|`@ComponentScan`]]

- serves as a shortcut for configuring the essential aspects of the application, making it a convenient entry point
- signals to the Spring Framework that the annotated class is the main class responsible for launching the application
