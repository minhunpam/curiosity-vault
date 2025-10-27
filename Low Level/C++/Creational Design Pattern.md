## Singleton Pattern #DesignPattern #Creational
- A singleton ensures a class has only one instance and provides a global access point to it
- A safe, modern, thread-safe implementation of singleton contain:
	- Private:
		- Constructor and Destructor
	- Public:
		- Explicitly delete copy constructor and assignment operator
		- A special method `static Singleton& getInstance()` which returns the singleton instance, creating it on first use
			- The `static` keyword for local variable means the variable is created only once (the first time the function is called), and it keeps its value for all future calls
		- Typically other non-static methods


## Builder Pattern #DesignPattern #Creational 
- designed to deal with the construction of comparatively complex objects
- when the complexity of creating object increases, the Builder Pattern can separate out the instantiation process by using another object (a builder) to construct the object
- This builder can then be used to create many other similar representations using a simple step-by-step approach

```java
public class BankAccount { 
	private String name; 
	private String accountNumber; 
	private String email; 
	private boolean newsletter; 
	
	// constructors/getters
	// constructor is also private so that only the Builder assigned to this class can access it
	private BankAccount(BankAccountBuilder builder) {
		this.name = builder.name;
		this.accountNumber = builder.accountNumber;
		this.email = builder.email;
		this.newsletter = builder.newsletter;
	} 
	
	public static class BankAccountBuilder { 
	// builder code 
	} 
}
```

```java
public static class BankAccountBuilder { 
	private String name; 
	private String accountNumber; 
	private String email; 
	private boolean newsletter;
	
	// Any mandatory fields are required as arguments to the inner class's constructor
	public BankAccountBuilder(String name, String accountNumber) { 
		this.name = name; 
		this.accountNumber = accountNumber; 
	} 
	
	// remaining optional fields can be specified using the setter methods
	public BankAccountBuilder withEmail(String email) { 
		this.email = email; 
		return this; 
	} 
	
	public BankAccountBuilder wantNewsletter(boolean newsletter) {
		this.newsletter = newsletter; 
		return this; 
	} 
	
	// the build method calls the private constructor of the outer class and passes itself as the argument. The returned BankAccount will be instantiated with the parameters set by the BankAccountBuilder
	public BankAccount build() { 
		return new BankAccount(this);
	} 
}
```

```java
BankAccount newAccount = new BankAccount
	.BankAccountBuilder("Jon", "123456789")
	.withEmail("jon@example.com")
	.wantNewsLetter(true)
	.build();
```

### When to use Builder Pattern
1. When the process involved in creating an object is extremely complex, with lots of mandatory and optional parameters
2. When an increase in the number of constructor parameters leads to a large list of constructors
3. When client expects different representations for the object that’s constructed