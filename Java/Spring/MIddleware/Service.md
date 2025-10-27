## `@Service`
- specialization of [[IOC Container#`@Component` - placed on a class|`@Component`]] -> automatically detected during class-path scanning
- used to indicate that a class belongs to the service layer in an application
- service layer typically contains the business logic of the application
- intermediary between **Controller Layer** and **Repository Layer**

## Why do we need service layer
 1. **Separation of Concerns**
    - Controller should only deal with web/HTTP stuff.
    - Repository should only deal with persistence/DB stuff.
    - Service layer sits in between → keeps things clean and maintainable.
2. **Business Logic Lives Here**
    - Example: "When a student enrolls in a course, also check seat availability and send a confirmation email."
    - That’s not DB logic, and not HTTP logic → belongs in Service.
3. **Reusability**
    - Multiple controllers (REST, web, scheduled jobs) can call the same service method.
    - No duplicate logic scattered around.
4. **Transaction Management**
    - You often annotate service methods with `@Transactional`.
    - Ensures all DB operations inside succeed or all roll back.
5. **Testability**
    - You can test your business logic independently (unit tests on Service) without spinning up a web server or touching the DB.
6. **Security & Validation**
    - You can put role-based access checks or input validation in service methods before hitting the database.