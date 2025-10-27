## What is an API first?
- Application Programming Interface
- most basically, is a mechanism that enables an application of service to access a resource within another application, service or database
	- **Client**: the application or service that accesses resources
	- **Server**: the application or service that contains the resources

## 6 Architectural constraints (REST design principles)
1. **Uniform interface**
	- All API requests from the same resource ( aka **uniform resource identifier (URI)**) should look the same, no matter where the request comes from
2. **Client-server decoupling**
	- client and server applications must be completely independent of each other
	- client application should only know the URI of request resource
	- server application shouldn't modify the client application other than sending it the requested data via HTTP
3. Statelessness
	- REST APIs do not require any server-side sessions 
		- **session** = set of data stored on the server that represents the state of a particular client (what did they request? How were they after certain requests?) between multiple requests
4. Cacheability
	- Resources should be cacheable on the client or server side (if possible)
	- Server responses also need to contain information about whether caching is allowed for the delivered resource
5. Layered system architecture
	- calls and responses go through different layers
	- need to be designed so -> neither the client nor the server can tell whether it communicates with the end application or an intermediary
6. Code on demand
	- sends static resources, but in certain cases, responses can also contain executable code (such as Java applets). In these cases, the code should only run on-demand

## How REST APIs work
- communicate through HTTP requests -> perform standard database functions: ***CRUD*** with resources
	- Create (POST) -> create a new record
	- Read (GET) -> retrieve a record
	- Update (PUT) -> update a record
	- Delete (DELETE) -> delete a record

- The state of a resource at any particular instant/ timestamp is known as the **resource representation**, which can be delivered to a client in virtually any format
	- instant = very specific moment in time
	- JavaScript Object Notation (JSON) is popular because it's readable by both humans and machines -> **programming language-agnostic (be able to be used by a lot of programming languages)**
- Request headers and parameters are also important in REST API calls because they include important identifier information (metadata, authorizations, URI, caching, cookie)

![[Java - client_server_communication.jpeg]]

## Serialization and Deserialization #Jargon #RestAPI 
- Serialization = converting Java objects into JSON for the client
- Deserialization = JSON -> Java objects

## Difference between `PUT` and `PATCH` Request
- HTTP `PUT` request is used to replace and update the entire resource or document
- HTTP `PATCH` request only updates the specific parts of that document
