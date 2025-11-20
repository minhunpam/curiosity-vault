[[JPA]]

- Spring Framework: a popular one to build Java applications
	- includes many layers that are responsible for specific purposes
	- is modular
	- BUT requires a lot of configuration
- Spring Boot: simplifies Spring development by providing sensible defaults and ready-to-use features

## Creating a Spring Boot project (in case you don't use Intellij Idea Ultimate)
- [spring initializr](https://start.spring.io/)
- Choose package types: [[Package Types#JAR (Java Archive)|Package Types]]

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


