## Architecture of [[A0 - Overview|OOP2-A0]]
### Front-end
- **Purpose**: user-facing part of the app
- **Technology**: HTML
- **Role**:
	- where users interact with the service
	- gathers inputs from the user (map coordinates/ routing requests) -> send them to middleware
	- displays the results returned by the system 

### Middleware
- **Purpose**: bridge between the front-end and back-end
- **Technology**: Java Spring (Spring Boot)
- **Role**: 
	- **Request Processing**: When frontend sends a request (for route calculation or fetching map data), the middleware processes it.
	- **Validation and Error Checking**: Ensures the request is valid, and if not, generates appropriate error messages
	- **Data Preparation**: can perform additional processing on the data (filtering, transforming) before forwarding it to the back-end

> Avoid adding `@ComponentScan` annotation to either Middleware or Back-end

## Back-end
- **Purpose**: Core data processing layer
- **Technology:** Java 
> No Spring Boot allowed in the Backend!

- **Role**:
	- Where the actual map data (roads, amenities, land usage) is stored and managed
	- The back-end receives requests from middleware -> process them (querying, calculating routes), -> send back the correct data to the middleware for user display
	- Back-end is a server implemented using Java Classes

