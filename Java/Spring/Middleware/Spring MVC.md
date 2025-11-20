## `ResponseEntity<T>`
- **represents the whole HTTP response: 
	- Status code 
	- Header
	- Body
	--> use it to fully configure the HTTP Response
-  We use `ResponseEntity`, because it gives complete control over your HTTP response, whereas returning a plain object (or using `@ResponseStatus`) only lets you control the **body** and sometimes the **status**, but not **headers** or dynamic conditions

### When should we use `ResponseEntity`?
1. You need to set a custom status code
2. You want to include custom headers
3. You want dynamic control over response

But when controller endpoints simply return JSON and uses standard success semantics -> no need to use `ResponseEntity`, because we have **Jackson** be responsible for that already


