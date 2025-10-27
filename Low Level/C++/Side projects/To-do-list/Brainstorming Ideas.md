- A To-do-list has a To-do-list Interface (made it **singleton**)
	- This Interface will encapsulate all components of the app
- In the app, user can create multiple to-do-lists
	- A to-do-list has following attributes:
		- date of creation
		- list of tasks
		- to-do-list name
- A to-do-list can have multiple task
	- A task has following attributes:
		- date of creation
		- name of the task
		- priority of the task (scale from 1 - 10) <**this can later be used to sort the list**>
- Because this is a console app, so it requires user input which processed by `CommandParser` class
	- I want to apply the Command Design Pattern this one, which means when user wants to execute a command, a concrete class for that command will be responsible
- Which commands does a to-do-list app need?
	- Create to-do-list (`CreateTODOListCommand`) ("create to-do-list [to-do-list-name] ")
	- Delete to-do-list (`DeleteTODOListCommand`) ("delete to-do-list [to-do-list-name]")
		- There will be duplicate to-do-lists
	- Create a task (`CreateTaskCommand`)
	- Delete a task (`DeleteTaskCommand`)
	- Sort a to-do-list (`SortTODOListCommand`)
		- + "less important"
		- + "more important"
	- copy an existing task (`CopyTaskCommand`)
	- quit the program (`QuitCommand`)
	- show instructions (`HelpCommand`)
	
	
- There are 2 states between `to-do-list` and `task` :
	1. inside a to-do-list --> can only create a task
	2. outside a to-do-list --> can only create a to-do-list
	 --> Can add a flag signify 2 states --> can instead use 1 class for `Create` or `Delete` instead of 2 separate classes for each feature
- 