Questions:
- Should we create a separate controller for 3 new requests/ endpoints
- 
## Request: `GET /usage`

- Return Format: A class that has:
	- area of the bbox - `double`
	- usages: (un-ordered) - `List<Landuse>`
		- `Landuse`
			- `String` type
			- `double` share
			- `double` area
		> **In case there is no land usage within the bbox --> this list should be empty**

1. Bounding Box Transformation
	- Validate the coordinate using `ErrorHandler`
	- 
	- Create an `Envelope` from given coordinates, then transform it using `CoordinateTransformer` created by Sandro
	- After the transformation, can instantly `getWidth` and `getHeight` of the envelope to calculate its area
2. Area Intersection and Calculation
	- When a bbox is created, there 2 types of Geometry/area:
		1. Ones that lie within the bbox
		2. Ones that intersect with the bbox
	- Fetch geometries (amenities/roads) that intersect with the envelope
	- Create a `HashMap<String, Double>` 
		- `String` - represents the landuse
		- `Double` - represents the area
		- Traverse through lists of amenities/ roads --> look at the `landuse` tag
			- if `landuse` hasn't existed in the hash map, add it
			- otherwise, keep adding area to the matching landuse
	- Once everything is done, create `Landuse` objects:
		- type and share can be fetched directly from the hash-map
		- $share = \frac{\text{area of intersection}}{\text{total bbox area}}$
		- add to final list (**sort by `share` attribute in ascending order**)

- The problem with my current approach is that I only look into constructed amenities and roads that have tags `landuse` but look into geometries that are not amenities and roads and specifically used as `landuse`

### Different approach
- Like amenities and roads, I have to create another JTS model that could be used to construct:
	- `Long` id 
	- `String` type
	- `Map<String, String>` tags
	- `Geometry` geom_raw
	- `Geometry` geom_transformed
- Because this endpoint requires query based on the coordinates --> store in the `MapDataStore` another `STRtree` for landuses and of course like amenities and roads --> have to add a helper function `List<LanduseJTS> getLanduseByCoordinates(Envelope envelope)`
- like Amenity and Road, landuse can also be constructed from
	- Nodes
	- Ways
	- Relations
> **Important thing: if a node, way or relation **

## Routing
- Create `RoutingController`
- Graph should be weighted by either travel time or road length (parameter-dependent)
	> **For travel time, use "speed" tag, default 30km/h**
- Build a graph with:
	- Edge - only `highway`-tagged roads
	- Vertices - their junctions
- Junction nodes
	1. First node of a way, last node of a way
	2. Shared by >= 2 highway ways
		- Create a map (k: id, v: node counter)
		- Traverse through the way node references and add it up

### Edge cases
- Start id and Finish id are the same --> still valid 200 --> 
	- length: 0
	- time: 0
	- roads: []

Questions:
- In order to run the `GET /route` endpoint, from I observed from the frontend, you have to run the `GET /roads` first. From the frontend perspective, running the `GET /route` would be impossible. But from the backend perspective, user still could make the request, then in this case, would the code be `400` right?


## Encounter the problem
• Here’s the short playbook I followed so you can repeat it next time:
  - Tried to start backend with `mvn -pl backend exec:java`.
  - It failed to start because port 8020 was already in use.
  - Confirmed something was listening: `ss -ltnp | rg ':8020\\b'`.
	  - List all listening TCP ports, show them numerically, and tell me which process owns each one.
	  - Show me which process is **listening on TCP port 8020**, including its PID and name.
  - Identified the process (java pid shown in ss output).
  - Killed it: kill `<pid>`.
  - Re‑ran mvn -pl backend exec:java and confirmed it was listening on 8020.

  If you hit it again, the key is to free port 8020 or change the port via JMAP_BACKEND_PORT.
