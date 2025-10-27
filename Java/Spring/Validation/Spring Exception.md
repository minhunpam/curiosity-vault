## `@ControllerAdvice`
- enable global exception handling across your entire Spring MVC application
- acts as a global handler for exceptions thrown by any controller method

### How does `@ControllerAdvice` work?
- group and manage multiple exception handlers in one global location
- uses `@ExceptionHandler` to map specific exceptions to the methods that handle them
#### How it Works:

1. ***Exception Occurs:*** When an exception is thrown during the execution of a controller method, Spring looks for an appropriate `@ExceptionHandler` method in `@ControllerAdvice`.
2. ***Exception Matches:** If the exception matches the type handled by the `@ExceptionHandler`, that method is executed.
3. ***Custom Response:** The `@ExceptionHandler` method returns a custom error response (e.g., an error message, status code, or data).

### `@ExceptionHandler`
- methods annotated with this annotation can be added to any controller to specifically handle exceptions thrown by request handling methods in the same controller
- Such methods can:
	1. handle exceptions without the `@ResponseStatus` annotation
	2. redirect the use to a dedicated error view
	3. build a totally custom error response
- Or such methods can be included in classes annotated with `@ControllerAdvice`

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
```
- tells Spring:
> Whenever a `MethodArgumentNotValidException` is thrown in any controller, run this method to handle it.

### `@ResponseStatus`
- Used to **define the HTTP status code** that should be returned to the client **when a specific exception is thrown**/ when a method executes successfully
#### 1. When used above an exception class
- annotate a custome exception class so that **whenever is's thrown**, Spring automatically returns the defined status code
```java
@ResponseStatus(HttpStatus.NOT_FOUND)  
public class NotFoundException extends RuntimeException {  
    public NotFoundException(String message) {  
        super(message);  
    }  
}
```

#### 2. When used above an exception handler method (`@ExceptionHandler`)
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public String handleResourceNotFound(ResourceNotFoundException ex) {
        return ex.getMessage();
    }
}
```
- When `ResourceNotFoundException` is thrown, the `handleResourceNotFound` is called -> the HTTP reponse will have **status 404 (Not found)**

#### ⚠️ BE CAREFUL!
- We don't need to set the response's status manually inside the `ResponseEntity` if we are fine with using `@ResponseStatus` above the function. HOWEVER, ***if `ResponseEntity`*** explicitly sets another status, that will ***override*** the `@ResponseStatus`

```java
@ExceptionHandler(ResourceNotFoundException.class)
@ResponseStatus(HttpStatus.NOT_FOUND)
public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
    return ResponseEntity.ok("Handled: " + ex.getMessage());
}
```

## Global Exception Handler Normalization #Jargon 
### Normalization
- The process of **transforming all different exceptions into a single consistent (normalized) response format**.
	- **Without normalization**: 
		- each part of your app might return different error structures (some JSON, some plain text, different status codes).
	- **With normalization**: 
		- all errors follow the **same schema** — same JSON fields, same response code structure, same logging strategy.