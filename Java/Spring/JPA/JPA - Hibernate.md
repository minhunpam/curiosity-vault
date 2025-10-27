- Hibernate is an **implementation of JPA specifications** and Java Framework that maps Java objects to database tables, which streamlines data persistence and retrieval without the need for complex SQL queries

## What is JDBC?
- **Java Database Connectivity**
- provides a set of Java APIs that enable programs to execute SQL statements and interact with any SQL database to access the relational databases from the Java program
- gives a flexible architecture to write a database-independent web application that can execute on different platforms and interact with different DBMS without any change

## ORM (Object-Relational Mapping) Tool
- simplifies the data creation, data manipulation and data access
- is a programming technique that maps the object to the data stored in the database
- internally uses the JDBC API to interact with the database
![[Java -Sender2.png]]

## How are H2 and Hibernate related to each other?
- H2:
	- a database (specifically in-memory SQL database written in Java)
	- stores your data and understands SQL queries
	- often used in Spring Boot projects for testing and quick prototyping
- Hibernate:
	- a JPA provider (an ORM framework)
	- job: map your Java objects (`@Entity` classes) to database tables and generate SQL queries automatically

## Why use Hibernate instead of talking to H2 directly?
- without Hibernate, you'd write raw JDBC:
```java
Connection c = DriverManager.getConnection(...);
PreparedStatement ps = c.prepareStatement("INSERT INTO todos ...");
ps.setString(1, "Buy milk");
ps.executeUpdate();

```

- with Hibernate, `repo.save(todo)` -> no plumbing code
- Also: your code doesn't care if the database is H2, Postgre, MySQL... Hibernate adapts

## Hibernate Validator 
- the de-facto standard for performing validation - the Bean Validation framework's reference implementation
### `@Valid`
- **When Spring Boot finds an argument annotated with _@Valid_, it automatically [[Jargon in tech#Bootstrapping Jargon|bootstraps]] the default JSR 380 implementation — Hibernate Validator — and validates the argument.**
- When the target argument fails to pass the validation, Spring Boot throws a `MethodArgumentNotValidException` exception



