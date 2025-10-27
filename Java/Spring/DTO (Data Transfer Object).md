- are objects that carry data between processes in order to reduce the number of method calls
- main purpose from the author of this pattern **Martin Fowler**
```
Reduce roundtrips to the server by batching up multiple parameters in a single call. This reduces the network overhead in such remote operations
```

-  Another benefit:
	- Encapsulation of the serialization's logic
		- By introducing a DTO, only the DTO is serialized into JSON
		- If you need to change formatting, you do it in the DTO mapping
		- **Your entity statys pure**, focused on persistence, not presentation
	-  Decoupling domain models from the presentation layer

## `record` keyword
### What is `record` in Java?
- is a special kind of class in Java designed to be a transparent, immutable data carrier
- When declaring a class as `record`, the compiler automatically generates:
	- `private final` fields
	- a constructor with all these fields
	- getters
	- `equals(...), hashCode(), toString()`
‼️ ***Avoid a lot of boilerplate you don't want to hand-write***
```java
public record Person (
	String name, 
	String address
) {}
```

### Why use `record` for DTOs?
DTOs are:
- **Immutable data carriers** (they just hold values, no behavior).
- Passed around between layers and serialized into JSON.

That’s exactly what `record` was designed for:
- **Immutable** → Once created, values don’t change (safer for multi-threaded apps).
- **Concise** → No need to generate boilerplate code.
- **Self-documenting** → You can see all fields at a glance in the header.
