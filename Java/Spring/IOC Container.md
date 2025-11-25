IOC = Inversion of Control -> your code does not control how dependencies are created and connected - the container does it for you
- IOC Container is set of objects that:
	- create objects (called **beans**)
	- manages their lifecycle
	- wires (injects) dependencies between them, based on your configuration file (like `@Component`, `@Service`, or XML files)
	- You just define your beans and their relationships

## `BeanFactory` & `ApplicationContext`
- interfaces `BeanFactory` and `ApplicationContext` ***represents the Spring IoC Container***
	- `BeanFactory` - root interface for accessing the Spring container -> provides basic functionalities for managing beans
	- `ApplicationContext` - sub-interface of `BeanFactory` 
		- offers all functionalities of `BeanFactory`
		- provides more enterprise-specific functionalities
			- resolving messages
			- supporting internationalization
			- publishing events
			- application-layer specific contexts
	⚠️ `ApplicationContext` - used as ***default Spring container***

### Types of `ApplicationContext`
1. `AnnotationConfigApplicationContext` - takes classes ***annotated with `@Configuration`, `@Component`***
```java
ApplicationContext context = new AnnotationConfigApplicationContext(AccountConfig.class); AccountService accountService = context.getBean(AccountService.class);
```


## `@Component` - placed on a class
- used to mark a class as a Spring-managed BEAN 
- When Spring scans the application, it detects classes annotated with `@Component` and registers them as BEANs in the Spring IOC container. These BEANs can then be injected into other components using dependency injection

### Specializations of `@Component`
1. `@Service` - Used for service-layer classes that contain business logic
2. [[JPA Repository|`@Repository`]] - Used for data access objects (DAOs) or classes that interact with databases
3. [[Java Annotations#`@Controler`|`@Controler`]] - Used for web controllers that handle user requests and return responses

### Bean name/ ID
```java
@Component("collegeBean")
```
- `collegeBean` is the **bean name/ id** that Spring assigns to this component in the application context
- by default, Spring will register the bean with a default name derived from the class name

## `@Configuration`
- indicates that a class contains `@Bean` definition methods, which the Spring container can process to generate Spring Beans for use in the application

## `@ComponentScan`
- used to specify the package that the framework should scan for Spring-managed components. which are annotated with:
	1. `@Component`
	2. `@Service`
	3. `@Repository`
	4. `@Controller`
- When Spring finds such classes, it automatically registers them as beans in the Spring application context
- The `@ComponentScan` is used along with the `@Configuration` -> eliminates the need for manual bean registration in XML files, making the configuration more concise and easier to manage
## `@Bean` - placed on method (typically) inside a `@Configuration` class
- used to define a method that produces a bean to managed by the Spring IOC Container
⚠️ Whenever you are using the `@Bean` annotation to create the bean you don’t need to use the `@ComponentScan` annotation inside your configuration class

### Customizing Bean Names
```java
@Bean(name = "myCollegeBean")
public College collegeBean() {
	return new College();
}
```

### Return type of a `@Bean` method
- A `@Bean` method can return ***any datatype that you want Spring to manage as a bean***

## Dependency Injection
### Example:
- Having `OrderService` class that handles order = when a order is placed, the payment from client needs to be processed = depends on a `PaymentService` (like `StripePaymentService`)
-> `OrderService` tightly depends on `StripePaymentService`

#### Problem:
- If tomorrow, we want to change from `StripePaymentService` to `PaypalPaymentService` , we will have to re-write our code for `OrderService` -> re-compile, re-test -> affects other classes that depend on `OrderService` 
- cannot test `OrderService` in isolation, as it is tightly decoupled to `StripePaymentService`

#### Solution:
- We want the `OrderService` to depend on `PaymentService` , that could be Stripe, Paypal or any providers
- `PaymentService` - interface -> like a contract that defines the capability of a class
- In this way, the `OrderService` is not affected and can be tested in isolation
- When we want to `StripePaymentService`, its implementation is given to `OrderService` <- ***DEPENDENCY INJECTION***

### Automatic Injection (Autowiring) of `TodoRepository` 

#### Automatic Injection (Autowiring)
- Spring finds the dependency and supplies it for you. You don't have to manually construct it

1. Without Spring 
```java
public class DemoDataRunner {
    private TodoRepository repo;

    public DemoDataRunner() {
        this.repo = new TodoRepositoryImpl(); // You create it manually
    }
}
```
- responsible for creating `new` `TodoRepository`
- if `TodoRepository` depends on something else (e.g. `Entitymanager`), you'd also have to construct and wire those by hand -> messy 

2. With Spring (***Automatic Injection***)
```java
@Bean
CommandLineRunner demoData(TodoRepository repo) {
    return args -> {
        repo.save(new Todo("Buy groceries"));
    };
}
```

- At startup, Spring scans for `TodoRepository`.
- Spring **creates a real implementation** of it (a proxy object backed by Hibernate).
- Then, when it sees `demoData(TodoRepository repo)`, it says:
    > “Oh, this bean needs a `TodoRepository`. I already have one — let me pass it in.”
    


