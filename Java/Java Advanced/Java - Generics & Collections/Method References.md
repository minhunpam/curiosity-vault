```java
@FunctionalInterface  
interface LearnToSpeak {  
    void speak(String sound);  
}  
  
class DuckHelper {  
    public static void teacher(String name, LearnToSpeak learner) {  
        learner.speak(name);  
    }  
}  
  
public class Duckling {  
    public static void makeSound(String sound) {  
//        LearnToSpeak learner = s -> System.out.println(s);  
        LearnToSpeak learner = System.out::println;  
        DuckHelper.teacher(sound, learner);  
    }  
}
```
- The `::` operator tells Java to call the `println()` method later
- A method reference and a lambda behave the same way at runtime
- Can pretend the compiler turns your method references into lambdas for you
- There are 4 formats for method references:

## 1. Static methods
```java
Consumer<List<Integer>> methodRef = Collections::sort;
Consumer<List<Integer>> lambda = x -> Colections.sort(x);
```

## 2. Calling Instance Methods on a particular object
```java
var str = "abc";
Predicate<String> methodRef = str::startsWith;
Predicate<String> lambda = s -> str.startsWith(s);
```

```java
var random = new Random();
Supplier<Integer> methodRef = random::nextInt;
Supplier<Integer> lambda = () -> random.nextInt;
```

## 3. Calling Instance Methods on a Parameter
```java
var str = "abc";
Predicate<String> methodRef = String::isEmpty;
System.out.println(methodRef.test(str));
```
- Java uses the parameter supplied at runtime as the instance on which the method is called
- Compared with the above instance example, even though they look similar,
	- one references a local variable name `str`
	- the other only references the functional interface parameters

## 4. Calling Constructor
- _constructor reference_ : is special type of method reference that uses `new` instead of a method, and it instantiates an object
```java
Supplier<List<String>> methodRef = ArrayList::new;
```
⚠️ It is common for a constructor reference to use a `Supplier`




