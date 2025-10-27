## `@RequestMapping`,`@GetMapping`, `@PostMapping`
- these annotations are used in controller classes to tell your application how to respond to different types of HTTP requests
### `@RequestMapping`
#### Class-level `@RequestMapping`
- Defines a **base URL path** (prefix) for all handler methods inside the controller
```java
@RestController
@RequestMapping("/students")
public class StudentController {

    @GetMapping
    public String getAll() {
        return "All students";
    }

    @PostMapping
    public String add() {
        return "Add student";
    }
}
```
- Here the base path is `/students`.
- `GET /students` → handled by `getAll()`.
- `POST /students` → handled by `add()`.
Without class-level `@RequestMapping`, you’d have to repeat `/students` on every method

#### Method-level `@RequestMapping`
- General-purpose mapping annotation
- can handle all HTTP methods
- can specify which HTTP method you want it to respond to with `method` attribute. 
	- **If not specified, it will match all HTTP method by default**
```java
@RequestMapping("/hello", RequestMethod.GET)
public String sayHello() {
	return "Hello from Spring";
}
// When a user visits `/hello` using GET method, run this function
```

### `@GetMapping`
- is used for mapping HTTP GET requests onto specific handler methods
#### Example:
```java
@GetMapping("/students")
public List<Student> getAllStudents() {
 return studentRepository.findAll();
}
```
- `getAllStudents` method will be called when an **HTTP Get request** is received at the `/students` URL

### `@PostMapping`
- maps specific URLs to handler methods allowing you to receive and process data submitted through POST requests

#### Example:
```java
@PostMapping("/addStudent")
public Student addStudent(@RequestBody String studentName) {
    return "Hello, "+studentName;
}
```
- `addStudent()` will be called when an HTTP Post request is received at the `/addStudent` URL
- Inside the `addStudent` method, we are also passing a parameter for Request Body to be sent while making a 

### What does the value inside (e.g. `"/"`) `@RequestMapping)` mean?
- The value inside `@RequestMapping` is called the **path** or **URL pattern**
- `@RequestMapping(...)` **at the class level** - sets the "base path"
- `"/"` -> root path of your application or controller
- is the "home" or "main" URL segment

## `@Controler` and `@RestController`
### `@Controler`
- annotation that marks a class as a **Spring MVC Controller**
- used to handle web requests
- by default -> returns views (like HTML pages - using `Thymeleaf`)

### `@RestController`
- special version of `@Controller`
- return data directly, not views (not `.html`)
- mainly used for REST APIs (returning JSON/ XML)

## `@RequestBody`
- It tells Spring:  _“Take the JSON (or XML, or whatever body content) from the **HTTP request body**, convert (deserialize) it into a Java object, and give it to my controller method.”_
- it is used in controller method parameters

### How it works under the hood
1. Client sends request with JSON Body
```http
POST /students
Content-Type: application/json

{
  "id": 1,
  "name": "Anna"
}
```
2. Spring MVC receives the request and "triggers" matching method to the request, whose parameter is annotated with `@RequestBody`
3. Spring then uses **HttpMessageConverter** (e.g Jackson for JSON) to deserialize the request body into a Java object
4. Java object is passed to your method -> now you can work with the object directly

## `@PathVariable`
### What does it do?
- binds a placeholder in the URI path to a method parameter in your controller
- used when part of the URL Path is dynamic

### Example
Controller
```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id) {
        return "User with ID = " + id;
    }
}
```

Request
```bash
GET /users/42
```

What happens?
- `{id}` in `@GetMapping("/{id}")` is a placeholder
- Spring sees `@PathVariable Long id` and extracts `42` from the path
- The method receives `id = 42`
