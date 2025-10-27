## `JpaRepository`
-  is a JPA specific extension of `Repository`. It contains the full API of `CrudRepository` and `PagingAndSortingRepository` -> contains API for basic CRUD operations and also API for pagination and sorting.

```java
public interface JpaRepository<T,ID>   
extends PagingAndSortingRepository<T,ID>, QueryByExampleExecutor<T>
```
Where:
- `T`: domain type that repository manages (Generally the Entity/ Model class name)
- `ID`: Type of the id of the entity that repository manages (the wrapper class of you `@Id` that is created inside the Entity/ Model class)

## What is a Repository and why do we need that? 
- Think of the **Repository** as the **translator** between your Java world (objects) and your Database world (tables/rows)
- With Spring Data JPA Repository:
	-  You declare an interface -> Spring auto-generates all the boring CRUD code at runtime
	- You describe what you want and Spring builds the SQL
## Naming Convention for Interface Method for Query Derivation 
⚠️ **Query Derivation**: the method name itself encodes the query. You don't write SQL

### Example with `findAllByOrderDueDateAsc`
- `findAllBy` → return all rows.
- `OrderBy` → tells Spring “apply ordering”.
- `DueDate` → match the field `dueDate` in the entity.
- `Asc` → ascending order (could also use `Desc`).

```sql
SELECT * FROM todos ORDER BY due_date ASC;
```

### At which one of the two does JPA look at (Java property names/ database column names)
-> Answer: Java property names
Example: `List<Todo> findAllByOrderDueDateAsc();`
- Spring matches **`DueDate`** to the **Java field/property name** in your entity class — _not_ the column name in the database

### ✅ Best practice
- Use ***camelCase*** in your POJO/entity fields (`dueDate`).
- Map them to ***snake_case*** DB columns with `@Column(name="due_date")`.
- Keep repository method names consistent with the **Java property name** (`dueDate`)

### Why do we make repositories as interfaces
- Because **we don’t want to write the boring code** ourselves.  
- We just **describe what we want**, and Spring writes the implementation for us
#### ✅ Why not just use a class?
- If we made a class, we’d have to **manually code everything** (`EntityManager`, SQL, mapping).
- With an interface, Spring can **generate it all** just from method names.
- This makes our code **shorter, cleaner, and easier to maintain**.

## `save()` Method
