[[JPA]]

- Spring Framework: a popular one to build Java applications
	- includes many layers that are responsible for specific purposes
	- is modular
	- BUT requires a lot of configuration
- Spring Boot: simplifies Spring development by providing sensible defaults and ready-to-use features

## Creating a Spring Boot project (in case you don't use Intellij Idea Ultimate)
- [spring initializr](https://start.spring.io/)
- Choose package types: [[Package Types#JAR (Java Archive)|Package Types]]

## Maven
- a build automation and project management tool
- simplifies the process of building, managing dependencies, and packaging projects by using a standardized project structure and config file (`pom.xml`)

## Project Structure
- `src`
	- `main`
		- `java` - where we have our java file
			- we have one preset java file which is our entry point to our application
		- `resources` - where we have our non-java files
			- `application.properties` - configuration file
	- `test` - where we write automated test
	
- `.idea` - contains configuration files
- `.mvn` - part of Maven wrapper, which is a way to run Maven without requiring it to be installed globally on your machine -> ensure consistent Maven build across environments
- `pom.xml`
	- stands for: project object model
	- core configuration file for Maven (⚠️ Without `pom.xml` , Maven can't build or run your project)
	- what does `pom.sml` do?
		- manages dependencies
		- build instructions
		- project information
		- plugins
		- profiles


## Dependency Management
### Dependency?
- is something your code relies on - like a library, framework, or tool

- `spring-boot-starter-web` ([Maven Central](https://central.sonatype.com/))
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>4.0.0-M1</version> 
</dependency>
```
- we can omit the version tag as its grandparent will specify the version which makes thing easily compatible ***Best Practice***
	- it simplifies the `pom.xml` file
	- if we upgrade the version of Spring Boot, all dependencies will be updated to a compatible version

### Starters
- set of convenient dependency descriptors that you can include in your application
- contain a lot of the dependencies are needed to get a project up and running quickly with a consistent, supported set of managed transitive dependencies
- naming pattern:
```
spring-boot-starter-*
```

### Types of dependencies: Direct and Transitive
1. **Direct**:
	- ones that we explicitly include in the project by using `<dependency>` tags
```xml
<dependency> 
	<groupId>junit</groupId> 
	<artifactId>junit</artifactId> 
	<version>4.12</version> 
</dependency>
```

2. **Transitive**:
	- are required by direct dependencies
	- Maven automatically includes required transitive dependencies in our project
	- can list all dependencies in the project using `mvn depedency:tree`

### Dependency Scopes
- Dependency scopes can help to limit the transitivity of the dependencies
- They also modify the classpath for different build tasks

1. Compile (default)
	- Dependencies with this scope are available on the classpath of the project in all build tasks
2. Provided
	- We use this scope to mark **dependencies that should be provided at runtime by JDK or a container**
3. Runtime
	- **The dependencies with this scope are required at runtime**
	- don’t need them for the compilation of the project code
	- JDBC driver is a good example of dependencies that should use the runtime scope
```java
<dependency> 
	<groupId>mysql</groupId> 
	<artifactId>mysql-connector-java</artifactId> 
	<version>8.0.28</version> 
	<scope>runtime</scope> 
</dependency>
```
4. System
	- very similar to `provided` scope
	- main difference is that `system` requires us to directly point to a specific jar on the system
	- ***building the project with system scope dependencies my fail on different machines if dependencies aren't present or are located in a different place than the one systemPath points to**

## Spring MVC (Model- View -Controller)
- Model - represents the entity (table) that would be used for the database
* View - what users see (front-end)
-  Controller - traffic controller, handles incoming requests
	- interact with the model to get the data and tell the view what to display

## Automating Restarts 
### Adding `devtools` dependency to `pom.xml`
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-devtools</artifactId>  
    <optional>true</optional>  
</dependency>
```

- In Settings
	- `Compiler` -> `Build project automatically`
	- `Advanced Settings` -> `Compiler` -> `Allow auto-make to start even if developed appplication is currently running`

## What are JAR and an executable JAR
- JAR - Java Archive - a package file format that bundles together all your Java classes and resources
- An executable JAR - special kind of JAR file that:
	- contains your program's code (all class files, resources, etc.)
	- includes a "main" class - starting point of your application
	- can be run directly

#### Use case:
- easily run Java application without setting up lots of stuff
- Spring Boot apps are often shipped as executable JARs -> can start the app with a single command

#### How to use:
```bash
java -jar <your-java-app-name>-1.0.0.jar
```

## Structuring your code
- you should locate your main application in a root package above classes
- The [[Java Annotations#`@SpringBootApplication` | `@SpringBootApplication]] is often placed in on your main class
```com
+- example 
	+- myapplication 
		+- MyApplication.java 
		| 
		+- controllers
			+- CustomerController.java
			+- OrderController.java 
		+- models
			+- Customer.java
			+- Order.java
		+- services
			+- CustomerService.java
			+- OrderService.java
		+- repository
			+- CustomerRepository.java
			+- OrderRepository.java
```

## What does the word ***artifact*** mean?
### In general software context
- an **artifact** is any file that is the result of a build process
- Think of it as **the output product of your code compilation/build**

### Artifact in Java/ Spring
- artifact refers to a **compiled package** of your project can be published and reused
- Typically a `.jar`, `.war`, or `.ear` file
- Identified by a unique set of coordinates:
	- **groupId**
	- **artifactId**
	- **version**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>3.3.2</version>
</dependency>
```
-> The artifact is `spring-boot-starter-web-3.3.2.jar`

### How is artifact different from dependency
- Dependency is ***artifact that your project needs*** in order to compile, run, or test
- Maven resolves the dependency by downloading the corresponding artifact from a repo
- Back to the above code snippet, `spring-boot-starter-web` is a **dependency** from the project's point of view
- Maven fetches the **artifact file (`spring-boot-starter-web-3.3.2.jar`)** to satisfy that dependency

## `CommandLineRunner`
- an interface that can be used to execute code after the Spring application is fully initialized but before the application is completely started
- any bean implementing this interface will have its `run` method invoked with the command-line arguments passed to the application

### When to use `CommandLineRunner`
1. Data Initialization - populate the database with initial data
2. Configuration Checks - verify that certain conditions or configurations are met before the application starts
3. Task Scheduling - schedule or initiate background tasks that need to start with the application

## Spring Boot Profiles
- A profile represents a specific set of configuration values, beans, and other settings tailored to a particular environment
- 