## Notes:
- Logger:
	- `middleWareStartup()`
	- `backendStartup()`
	- `backendLoadFinished()`
	- `backendLogAmenitiesRequest()`
	- `backendLogAmenityRequest()`
	- `backendLogRoadRequest()`
	- `backendLogRoadRequest()`
## Parse the Dataset - OSM

- Parser for  XML: SAX Parser
	- Why?
		- event-driven parser that processes XML data as a stream, without loading the entire document into memory, especially for large files
		- In the SAX parser, data extraction typically happens within the `startElement()` and `endElement()` methods, where you can retrieve attribute values and prepare values and prepare data for your application
- Processing data:
	- JTS (Java Topology Suite) **highly recommended using it to handle the geometric and spatial data efficiently**
- Middleware and Backend communication:
	- Implement gRPC into your service to enable communication between the Middleware and the Backend
		- allowing the Middleware to request actual map and geospatial data (processed and stored in the Backend)
	- Built-in features of gRPC:
		- Protocol Buffers: gRPC uses Protobuf to define messages and services, saving you from writing serialization/ deserialization code
		- Code generation: gRPC auto-generates client and chat server code from service definitions
		- Client features: gRPC provides built-in features like connection pooling, load balancing, retries, and multiplexing over HTTP/2

## Project Structure


## Tasks
### Data Loading
-  Download the Dataset from the OSM (OpenStreetMap) file
	- put into `/data`
	- ***DO NOT COMMIT AND PUSH THE OSM FILE(S)***
#### Loading Nodes (`node`)
- consists of a single point in space
	- latitude - X-coordinate
	- longitude - Y-coordinate
	- node id
	- store tags associated with each node in dictionary
- Implementing the parsing `.OSM` file inside `MapServiceServer`
##### Dom Approach
- Testing
```
 mvn -pl backend -Dtest=MapServiceServerTest test
```



