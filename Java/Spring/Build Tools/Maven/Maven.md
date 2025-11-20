- a build automation and project management tool
- simplifies the process of building, managing dependencies, and packaging projects by using a standardized project structure and config file (`pom.xml`)

## Maven `pom.xml`
- **POM <-> Project Object Model**
- Core configuration file of a Maven Project defining
	- project metadata
	- dependencies
	- plugins
	- build configurations required to
		- test
		- compile
		- package
		- deploy the application

### Major Sections of `pom.xml`
1. **Project information (`<project>`)**
	- root element of the POM file
	- contains all project-related metadata and configuration to manage the build process
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
```
2. **Project coordinates**
	- defines unique identifiers for your project in a Maven repository
```xml
<groupId>com.geeks</groupId>
<artifactId>maven-pom-example</artifactId>
<version>1.0-SNAPSHOT</version>
```
- **groupId:** Usually your organization’s domain in reverse
- **artifactId:** Project or module name
- **version:** Current version of the project

3. Properties Section
```xml
<properties>
    <java.version>1.8</java.version>
    <selenium.version>4.6.0</selenium.version>
    <testng.version>7.4.0</testng.version>
</properties>
```
- defines reusable variables to maintain consistency and simplify version updates

4. Dependencies Section
```xml
<dependencies>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>${selenium.version}</version>
    </dependency>
</dependencies>
```
- List all external libraries your project depends on
- Maven automatically downloads them from configured repositories
- Each dependency includes:
	- **groupId:** Library’s organization (in reverse)
	- **artifactId:** Library name
	- **version:** Library version

5. Repositories Section
	- Defines remote repositories from where Maven  retrieves dependencies
```xml
<repositories>
    <repository>
        <id>central</id>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
</repositories>
```
- If no repository is specified, Maven defaults to Maven Central

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

## [[Maven Packaging Types]] (`<packaging></packaging>`)

