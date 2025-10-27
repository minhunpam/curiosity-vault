## `@DataJpaTest`
- this annotation invokes several Spring Boot auto-configuration that are used in our application
- it closely replicates the real behavior of our application
- also allows us to customize these configurations if necessary

- Some of the features are:
	- scanning of our `@Entity` classes
	- configuring the Spring Data JPA repositories
	- enables the logging of the database queries that are executed
- **all tests decorated with the _@DataJpaTest_ annotation become transactional**
- 