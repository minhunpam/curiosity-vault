## What is H2 Database?
- a lighweight and fast SQL database written in JAVA
- can run in 2 modes:
### In-memory Mode
- particularly useful for testing and development because it allows you to create a temporary database that is automatically destroyed when the application stops

### Embedded Mode
- is used for application that need a small, self-contained database

### You can configure H2 Database in `application.properties` or `application.yml` (YAML file), but what is the difference? #JavaCases

Even though they serve the same role (central config), here is the difference

1. `application.properties`
	- Format: simple `key=value` pairs
	- Easy for small configs
	- Becomes messy as soon as you have structured data (nested configs, profiles, lists)
2. `application.yml`
	- Format: YAML (indentation shows hierarchy)
	- More readable for hierarchical configs
	- ***Real-world Practice!!!*** - easier to organize and read

- Both files can exist, but values in `application.properties` override those in `application.yml`.
- For clarity and maintainability, pick one.