## `@RequestParam`
- Used to extract query parameters, form parameters, and even files from the request
### Attributes
- `name` - configure name
- `required`
	- method parameters annotated with `@RequestParam` are required by default. This means that if the parameter isn't present in the request, we'll get an error
	- we can configure: `required=false`
	- 