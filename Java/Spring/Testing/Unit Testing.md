## What is Unit Testing?
- focus on one part of an application in total isolation (usually: single class or function)
- 

## Rule of thumbs: You don't have to test everything
- would likely to unit test things that are large and prone to changes and breaking

## Pattern for writing good tests: Arrange - Act - Assert
1. **Arrange** inputs and targets
	- set up the test case
	- does the test require any objects or special settings?
2. **Act** on the target behavior
	- cover the main thing to be tested
		- calling a function/ method
		- calling a REST API
		- interacting with a web page
	- keep actions focused on the target behavior
3. **Assert** expected outcomes
	- _Act_ steps should elicit some sort of response
	- _Assert_ steps verify the goodness or badness of that response
	-> Assertions will ultimately determine if the test passes or fails

## Naming Convention for Unit Test
```java
public String PokemonRepository_SaveAll_ReturnsSavePokemon() {}
// PokemonRepository: name of part of the app that you are testing with
// SaveAll: name of the function to be tested
// ReturnsSavedPokemon: what you plan that the test to do when it passes
```