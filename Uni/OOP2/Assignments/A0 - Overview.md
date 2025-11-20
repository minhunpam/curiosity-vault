## Objectives:
- creating simple Java objects -> returning them using Spring Boot API
- implementing basic error-checking routines
- implementing several `GET` requests that respond with mock data (roads and amenities)
	- follow REST principles (stateless)
	- each URL (endpoint) represents a specific resource

## Build tool:
- Maven
- `pom.xml` file has been all set up (dependencies, plugins and build routines)
> ***not allowed to add any dependencies in the `pom.xml` files***

## [[System Design#Architecture of A0 - Overview OOP2-A0|Architecture]]

## Basic Concept:
- implement a _RESTful_ API using Spring Boot REST framework to build a simple, versatile mapping service

> ***Not required to use the MVC pattern for this assignment, but should understand how to structure and handle API requests effectively and check for errors***

## Structure
### Middleware:
-  starts the Spring Boot application + handles environment variables (`MapLogger`)
#### Models (POJOs) (@Data)
- `Amenity`
- `Road
	- `Crs`
		- `CrsProperties`
	- `Geom`
#### Controller(s): (`@RestController`)
- wired to repository 
- handles `GET` requests of amenities and roads (annotated `@GetMapping)
#### Repository (-ies): (`@Repository`)
- holds the list of targeted objects
- create (dummy) objects based on the sample requests in JSON format as String
	- implementation: 
		- create an `ObjectMapper` then convert JSON string to real Entity

### Backend:
- `MapServiceServer`
- handles environment variables (`MapLogger`)

### Error Handling
- `400 Bad Request` - missing/ invalid parameters
- `404 Not found` - entity not found
- Bounding boxes:
	- bounds must ensure that all coordinates fall within valid **latitute** and **longitude** ranges
		- **latitude** $\in [0^o, 90^o]$ (North and South poles)
		- **longitude** $\in [0^o \text{ at Greenwich }, 180^o \text{ East and West}]$
	- top-left corner (`tl.x, tl.y`) should be lie north-west of the bottom-right corner (`br.x, br.y`)
- TCP ports
	- Uses 16-bit ports
	- Any value outside this interval should trigger a config-error
	- Keyword: _reserved ports_
- Amenities endpoint (`/amenities` )

#### Implementation:
- Create custom exceptions
- Create an exception handler


## Questions
1. Startup methods in `MapLogger` -> where to put them?
	- `middlewareStartup`
	- `backendStartup`
2. What does this line do? 
```java
String osm_file = System.getenv().getOrDefault("JMAP_BACKEND_OSMFILE", DEFAULT_BACKEND_OSMFILE);
```
- It tries to read an environment variable named `JMAP_BACKEND_OSMFILE` from your system
	- If that env var exists, its value is used
	- If it doesn't exist, it fails back to the default value defined in your code
