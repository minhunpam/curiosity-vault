- A package is used to group related classes (just like **folder in a file directory**)
- use package to avoid name conflicts and to write a better maintainable code
- Packages are divided into 2 categories:
	- Built-in packages (packages from the JAVA API)
	- user-defined packages (create your own packages)

## Built-in Packages
- library of prewritten classes
- library is divided into:
	- packages
	- classes
- To use a class or a package from the library, you need to use the `import` keyword:
```java
import package.name.Class; // Import a single class
import package.name.*; // Import the whole package
```

### Import a Class
```java
import java.util.Scanner;
```
- `java.util` - package
- `Scanner` - class of `java.util` package
- To use the `Scanner` class, create an object of the class and use any of the available methods

### Import a Package
```java
import java.util.*;
```

## User-defined Packages
- To create your own package, need to understand that Java uses a file system directory to store them. Just like folders on your computer
- To create a package, use the `package` keyword:
```java
package mypack;
class MyPackageClass {
  public static void main(String[] args) {
    System.out.println("This is my package!");
  }
}
```
- Compile the class first:
```bash
C:\Users\_Your Name_>javac MyPackageClass.java
```
- Then compile the package:
```
C:\Users\_Your Name_>javac -d . MyPackageClass.java
```
- this forces the compiler to create the "mypack" package.
- `-d` - specifies the destination for where to save the class file
- `.` sign - if you want to keep the package within the same directory
- ⚠️ The package name should be lower case to avoid conflicts with class names
To run the **MyPackageClass.java**
```bash
C:\Users\_Your Name_>java mypack.MyPackageClass
```

## Why can't interfaces/ classes in the same `.java` file with other class be marked `public` #JavaMustKnow 
```java
public interface Walk { ... }
public interface Run { ... }
public class Cat implements Walk, Run { ... }
```
-> Compiler Error

- Java’s compiler and class loader expect **each public type** to be in a file named exactly after it
- `Walk` would need to be in `Walk.java`, and `Run` in `Run.java`
- This convention makes locating source files easy and enforces a predictable project structure

## What can do
1. If you want `Walk` and `Run` interfaces in the same file as `Cat`, can remove `public` from their declarations
2. Put each `public` type in its own files

> ***One public top-level class/ interface/ enum per file, and the file name must match it***

