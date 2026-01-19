- `Scanner` class - get user input (found in `java.util` package)
```java
import java.util.Scanner;  // Import the Scanner class

class Main {
  public static void main(String[] args) {
    Scanner myObj = new Scanner(System.in);  // Create a Scanner object
    System.out.println("Enter username");

    String userName = myObj.nextLine();  // Read user input
    System.out.println("Username is: " + userName);  // Output user input
  }
}
```
- `System.in` is a standard input stream in Java (it represents keyboard input). Pass `System.in` so that the `Scanner` knows to read input from the keyboard

## `.isEmpty()` vs. `.isBlank()`
1. `.isEmpty()`
	- checks if a `String` has length 0
```java
"".isEmpty();          // true  (zero length)
" ".isEmpty();         // false (has one space character)
"abc".isEmpty();       // false
```

2. `isBlank()`
	- Checks if a `String` is **empty OR contains only whitespace characters** (spaces, tabs, newlines, etc.)
```java
"".isBlank();          // true   (empty)
" ".isBlank();         // true   (only a space)
"\n\t".isBlank();      // true   (newline + tab)
"abc".isBlank();       // false
```