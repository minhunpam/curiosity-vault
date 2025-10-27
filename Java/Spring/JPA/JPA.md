- [[JPA - Hibernate]]
- [[H2 Database]]
- [[JPA Repository]]
- [[Transaction]]

## JPA
- Java Persistence API
- is a Java specification that provides functionality and standards for ORM tools
- used to examine, control, and persist data between Java objects and relational databases
- is regarded as a standard technique for Object Relational Mapping

## What does the word `persist` mean in context of JPA?
- to make a transient entity instance managed and persistent == ***to store it in the database***
## POJO (Plain Old Java Objects)
- a straightforward type with no references to any particular frameworks
- A POJO has no naming convention for our properties and methods
```java
public class EmployeePojo {

    public String firstName;
    public String lastName;
    private LocalDate startDate;

    public EmployeePojo(String firstName, String lastName, LocalDate startDate) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.startDate = startDate;
    }

    public String name() {
        return this.firstName + " " + this.lastName;
    }

    public LocalDate getStart() {
        return this.startDate;
    }
}
```
This class can be used by any Java Program as it's not tied to any framework
## Entity
- An entity represents a **table stored in a database**
- Every instance of an entity represents a row in the table

### `@Entity`
- We have a [[JPA#POJO (Plain Old Java Objects)|POJO]] called _Student_ -> represents the data of a student and would like to store it in the database -> we should define an entity so that JPA is aware of it
- specify this annotation at the class level
⚠️ ***must ensure that the entity has a no-arg constructor + a PRIMARY KEY***

```java
@Entity(name="student")   // Entity name defaults to the name of the class
public class Student {
	// fields, getters and setters
}
```

### `@Id`
- defines the primary key
- We can generate the identifiers in different ways -> specified by the `@GeneratedValue(strategy=GenerationType.[...])`
	- [...] could be:
		- AUTO -> JPA provider will use any strategy it wants to generate the identifiers
		- TABLE
		- SEQUENCE
		- IDENTITY -> relies on the database's auto-increment feature

### `@Table`
- In most cases, **the name of the table in the database and the name of the entity won’t be the same**
```java
@Entity
@Table(name="STUDENT", schema="SCHOOL")
public class Student {
    
    // fields, getters and setters
    
}
```
- Schema name helps to distinguish one set of tables from another
- If we don’t use the _@Table_ annotation, the name of the table will be the name of the entity

### `@Column`
- is used to mention the details of a column in the table
- `@Column` annotation has many elements such as _name, length, nullable, and unique_
	- _name_ -> name of the column in the table (string) - if we don't specify this annotation -> the name of the column in the table will be the name of the field
	- _length_ -> length (int)
	- _nullable_ -> whether the column is nullable or not (boolean)
	- _unique_ -> whether the column is unique or not (boolean)
	
```java
@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    @Column(name="STUDENT_NAME", length=50, nullable=false, unique=false)
    private String name;
    
    // other fields, getters and setters
}
```

## `@Enumerated`
- use the _@Enumerated_ annotation to specify whether the _enum_ should be persisted by name or by ordinal (default) by using `@Enumerated(EnumType.String)`

```java
@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    @Column(name="STUDENT_NAME", length=50, nullable=false, unique=false)
    private String name;
    
    @Transient
    private Integer age;
    
    @Temporal(TemporalType.DATE)
    private Date birthDate;
    
    @Enumerated(EnumType.STRING)
    private Gender gender;
    
    // other fields, getters and setters
}
```

[[JPA Entity Lifecycle Events]]

