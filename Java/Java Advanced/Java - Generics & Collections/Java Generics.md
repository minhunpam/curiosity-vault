- allow you to write classes, interfaces and methods that work with different data types, without having to specify the exact type in advance
-> make your code more flexible, reusable, and type-safe

## Why?
- code reusability
- type safety
- cleaner code

## Generic class
```java
class Box<T> {  
  T value; // T is a placeholder for any data type  
  
  void set(T value) {  
    this.value = value;  
  }  
  
  T get() {  
    return value;  
  }  
}  
  
public class Main {  
  public static void main(String[] args) {  
    // Create a Box to hold a String  
    Box<String> stringBox = new Box<>();  
    stringBox.set("Hello");  
    System.out.println("Value: " + stringBox.get());  
  
    // Create a Box to hold an Integer  
    Box<Integer> intBox = new Box<>();  
    intBox.set(50);  
    System.out.println("Value: " + intBox.get());  
  }  
}
```

## Generic Method
```java
public class Main {  
  // Generic method: works with any type T  
  public static <T> void printArray(T[] array) {  
    for (T item : array) {  
      System.out.println(item);  
    }  
  }  
  
  public static void main(String[] args) {  
    // Array of Strings  
    String[] names = {"Jenny", "Liam"};  
  
    // Array of Integers  
    Integer[] numbers = {1, 2, 3};  
  
    // Call the generic method with both arrays  
    printArray(names);  
    printArray(numbers);  
  }  
}
```

## Bounded types
- can use the `extends` keyword to limit the types a generic class of method can accept
```java
class Stats<T extends Number> {  
  T[] nums;  
  
  // Constructor  
  Stats(T[] nums) {  
    this.nums = nums;  
  }  
  
  // Calculate average  
  double average() {  
    double sum = 0;  
    for (T num : nums) {  
      sum += num.doubleValue();  
    }  
    return sum / nums.length;  
  }  
}  
  
public class Main {  
  public static void main(String[] args) {  
    // Use with Integer  
    Integer[] intNums = {10, 20, 30, 40};  
    Stats<Integer> intStats = new Stats<>(intNums);  
    System.out.println("Integer average: " + intStats.average());  
  
    // Use with Double  
    Double[] doubleNums = {1.5, 2.5, 3.5};  
    Stats<Double> doubleStats = new Stats<>(doubleNums);  
    System.out.println("Double average: " + doubleStats.average());  
  }  
}
```

[[Method References]]

## Using the Diamond Operator
Before:
```java
List<Integer> list = new ArrayList<Integer>();
Map<String, Integer> map = new HashMap<String, Integer>();
Map<String, Integer> map = new HashMap<String, Integer>();
```
After `<>`:
- The diamond operator is a shorthand version that allows you to omit the generic type from the right side of a strategy when the type can be inferred it
```java
List<Integer> list = new ArrayList<>();
Map<String, Integer> map = new HashMap<>();
Map<String, Integer> map = new HashMap<>();
```

- The diamond operator cannot be used as the type in a variable declaration
```java
List<> list = new ArrayList<Integer>();     // DOES NOT COMPILE
Map<> map = new HashMap<String, Integer>(); // DOES NOT COMPILE
```