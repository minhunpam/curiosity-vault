## What is `OpenJDK` ?
- Open Java Development Kit - official open-source implementation of the Java Platform, Standard Edition (Java SE)
- include:
	- Java Compiler (`javac`)
	- Java Virtual Machine (`JVM`)
	- Standard class libraries (`java.util`, `java.lang`)
	- Development tools


## What is difference `Internal JDK` and `system-installed JDK`

| Purpose                        | IntelliJ Runtime JDK    | System-installed JDK |
| ------------------------------ | ----------------------- | -------------------- |
| Runs the IntelliJ IDE          | ✅                       | ❌                    |
| Compiles & runs Java code      | ❌ (unless configured)   | ✅                    |
| Used in IntelliJ terminal      | ❌                       | ✅                    |
| Needs to be installed manually | ❌ (comes with IntelliJ) | ✅                    |

## Install JDK with `javac`
```
sudo apt install openjdk-21-jdk
```
### After installation:
```
javac -version
java -version
```
- When there is a message recommendation: `openjdk-21-jdk-headless`, don't use this command, even though it contains `javac`, JVM, but it doesn't have GUI-related tools

