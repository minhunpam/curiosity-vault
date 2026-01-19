Divide modifiers into 2 groups:
- **Access Modifiers** - control access level
- **Non-Access Modifiers** - do not control access level, but provides other functionality

## Access Modifiers
### For classes:

| Modifier | Description                                             | Example             |
| -------- | ------------------------------------------------------- | ------------------- |
| `public` | the class is accessible by any other class              | `public class Main` |
| default  | class is only accessible by classes in the same package | `class Main`        |

## For attributes, methods and constructors
| Modifier    | Description                                   | Example |
| ----------- | --------------------------------------------- | ------- |
| `public`    | accessible for all classes                    |         |
| `private`   | accessible within the declared class          |         |
| `protected` | accessible in the same package and subclasses |         |
| default     | accessible in the same package                |         |

## Non-Access Modifiers
### For classes:
| Modifier   | Description                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------ |
| `final`    | cannot be inherited by other class                                                                     |
| `abstract` | cannot be used to create objects. To access an abstract class, it must be inherited from another class |
### For attributes, methods
| Modifier       | Description                                                                                                                                   |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `final`        | cannot be overriden/ modified                                                                                                                 |
| `static`       | belongs the class, rather than an object                                                                                                      |
| `abstract`     | can only be used in an abstract class, and can only be used on methods. The method does not have a body. The body is provided by the subclass |
| `transient`    | are skipped when serializing the object containing them                                                                                       |
| `synchronized` | can only be accessed by one thread at a time                                                                                                  |
| `volatile`     | the value of an attribute is not cached thread-locally, and is always read from the "main memory"                                             |
