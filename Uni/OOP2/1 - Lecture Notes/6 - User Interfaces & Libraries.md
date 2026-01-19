## Why GUI libraries?
- Encapsulate complex UI logic and components for reuse and maintainability
- Promote separation of concerns (UI vs. business logic) -> **Model View Presenter (MVP)**
- Facilitate rapid development with pre-built, tested components
- Improve consitency and scalability of user interfaces

## Model View Presenter (MVP)
**Model–View–Presenter (MVP)** is an **architectural pattern** used to separate **UI**, **business logic**, and **data**, making applications easier to test and maintain
1. **Model**:
	- Holds **data** and **business logic**
	- Knows nothing about the UI
2. **View**:
	- The **UI layer**
	- Displays data
	- Forwards user actions to the Presenter
3. **Presenter**:
	- The **middleman**
	- Handles logic and updates the View
![[OOP2 - MVP.png]]
- MVP flow:
	1. User clicks a button (View)
	2. View calls a method on the Presenter
	3. Presenter updates / queries the Model
	4. Presenter formats the result
	5. Presenter tells the View what to display
### Advantages and Disadvantages
1. **Advantages**
	- Clear separation of concerns (UI, logic, data)
	- Easier to test and maintain
	- Promotes code reuse and modularity
	- Facilities parallel development (UI vs. logic)
2. **Disadvantages**
	- More boilerplate code compared to simpler patterns
	- Can be overkill for small applications
	- Communication between components may become complex

## GUI libraries
- Historic: Wiring, punched cards
- Command Line Interface (CLI)
- Graphical User Interface (GUI):
	- WIMP (Windows, Icons, Menus, Pointer)
	- Others
- Natural user interface (NUI)
	- Touch, gestures, eye tracking, speech,...

## JavaFX
### Terminology:
- **Scene graph**:
	- Tree structure representing all UI elements
- **Stage**: A window
- **First stage**
	- The initial window given to an application as a parameter
	- Additional stages can be created on the fly
- **Scene**: a container to fill a stage; contains the graphical application content, e.g. widgets, shapes, subcontainers, etc.
 - **Layout**: how to layout the UI elements in a scene
- **Event**: 
	- Caused by user input (e.g., mouse click)
	- Handled by event listeners
	- Event provides details for the processing (e.g, mouse coordinates)
### JavaFX properties
- JavaFX uses **properties** to enable observability and binding
- Properties wrap values (e.g, `StringProperty` `IntegerProperty`)
- Support listeners for change notification
- Used for UI controls, models, and more

### Binding
- **Bindings** connect properties so that changes propagate automatically
```java
StringProperty text = new StringProperty();
Label label = new Label();
label.textProperty().bing(text);
```
- **Unidirectional binding**: One property observes another; changes propagate in one direction only.
- **Bidirectional binding**: Both properties observe each other; changes propogate in both directions
- Useful for synchronizing UI controls and model data
```java
label.textProperty().bind(textProperty);
textField.textProperty().bindBidirectional(model.textProperty);
```
### Observable Collections
- JavaFX provides `ObservableList`, `ObservableMap`, and `ObservableSet`
- Collections notify listeners about changes (add, remove, update)
- Used for dynamic UI elements (e.g., `ListView`, `TableView`)
```java
ObservableList<String> items = FXCollection.observableArrayList();
items.addListener(
	(ListChangeListener<String> change -> {
		while (change.next()) {
			if (change.wasAdded()) {
				System.out.println("Added: " + change.getAddedSublist());
			}
		}
	})
);
items.add("Hello");
```

### Threading and UI Access
- **JavaFXApplication Thread** is responsible for all UI updates and event handling.
- **Rule**: Only the JavaFX ApplicationThread may access or modify UI elements.
```java
Label label = new Label("Hello OOP2!");
//...
Thread t = new Thread(() -> {
	Thread.sleep(100);
	Platform.runLater(() -> {
		label.setText("Updated from background thread");
	});
});
```
- Use `Platform.runLater()`1 to schedule code on the JavaFX
- `javafx.concurrent`
	- `Worker`: Interface for background
	- `Task`: desgined for background computation
	- `Service`: Reusable wrapper for `Task`, manages task lifecycle and restartability
	- `ScheduledService`: extension of `Service` for periodic execution

## 8 Golden rules
1. **Strive for consistency**
	- Use familiar icons, terminology, and layouts
	- Maintain uniformity in colors, fonts, and actions
	- Predictable behavior reduces user confusion
2. **Seek universal usability**
	- Provide accelerators (e.g, keyboard shortcuts), but explanations for novoices
	- Allow customization for power users
3. **Offer informative feedback**
	- Give immediate, clear response to user actions
	- Use appropriate feedback for different actions
4. **Design dialogs to yield closure**
	- Group actions into meaningful sequences
	- Provide clear beginnings, middles, and ends
5. **Prevent errors**
	- Prevent errors where possible
	- Provide helpful, specific error messages
	- Allow easy recovery from mistakes
	_Example: Highlighting invalid fields in a form and suggesting corrections_
6. **Permit easy reversal of actions**
	- Support undo and redo functionality
	- Make actions reversible to encourage exploration
	_Example: Undoing the last drawing stroke in a graphics editor_
7. **Support internal locus of control**
	- Let users initiate and control actions
	- Avoid unexpected system behaviors
	_Example: Users choose when to save or submit their work_
8. **Reduce short-term memory load**
	- Minimize information users must remember
	- Use recognition over recall
	_Example: Autofilling address fields based on previous entries_





