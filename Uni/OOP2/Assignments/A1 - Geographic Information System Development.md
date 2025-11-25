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
	```java
	public static void backendLoadFinished(long nodes, long ways, long relations) {  
	    logger.info("Finished Loading: " + nodes + " nodes, " + ways + " ways, " + relations + " relations!");     
	}
	```
	- ***Log in the backend after finishing loading all the nodes (not longer than 5 seconds) and log the amoount of types NOT referenced by other entities*** 
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

#### Loading Ways (`way`)
- A way is an ordered list of nodes
	```xml
	<way id="96073570">
		<nd ref="1113132204"/>
		<nd ref="1113139110"/>
		<nd ref="7093560467"/>
		<tag k="access" v="destination"/>
		<tag k="highway" v="service"/>
		<tag k="service" v="driveway"/>
		<tag k="source" v="aerial imagery"/>
		<tag k="surface" v="asphalt"/>
	</way>
	```
 - Normally also has at least 1 tag OR participates in a relation
 - A way can have between 2 and 2000 nodes
 - **Faulty ways with zero or one node**
- Can be open or closed
	- If a way is closed (a loop) AND consists >2 nodes -> converted to `Polygon`
		- Question: closed? Does that mean list of nodes references refer to the same node for the first and the last
	- If a way is open -> constructed to `LineString`
- These ways can describe both amenities and roads
- ***If you do not find a node that is referenced by a way, you can just ignore it***
	- ***Ignore the non-existing node the way is referencing, but still try to build the geometry of the way with rest of the nodes***
	- ***If there is ONLY 1 node in a way then you can still get a geometry - just not necessarily a line***
		- If way with no existing node, then you have NEITHER a way NOR a node to reference
- Tags for each way -> Stored in a dictionary

#### Loading Relations (`relation`)
- ***There is 1 edge case that where relation references not only nodes/ways but also another relation*** -> log all relations
	```xml
	<relation id="12395081">
		<member type="way" ref="913322432" role="outer"/>
		<member type="way" ref="913322428" role="outer"/>
		<member type="way" ref="913322429" role="outer"/>
		<member type="way" ref="120082904" role="outer"/>
		<member type="way" ref="913322430" role="outer"/>
		<member type="way" ref="921519862" role="inner"/>
		<member type="relation" ref="12395080" role="outer"/>
		<tag k="landuse" v="forest"/>
		<tag k="source" v="GeoImage.at High-Res"/>
		<tag k="type" v="multipolygon"/>
	</relation>
	```
- A relation must have at least the `type=*` tag
	- And a group of members (an ordered list of one or more nodes, ways, and/or relations)
	- Used to define logical/ geographic relationships between these different objects
- Can have a `type="multipolygon"`
	- **inner** and **outer** structure
	- [[https://locationtech.github.io/jts/javadoc/org/locationtech/jts/geom/MultiPolygon.html|MultiPolygon in JTS]]
- If the relation is not a MultiPolygon, it is just a `GeometryCollection`
	- Always use `GeometryCollection`as the final type for relations
- can use the [[https://locationtech.github.io/jts/javadoc/org/locationtech/jts/operation/linemerge/LineMerger.html|LineMerger in JTS]] to help build these MultiPolygons
- For relations with the `type="building` -> Use the `outline`

### Communication with Backend 

