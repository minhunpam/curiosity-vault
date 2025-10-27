- are ways to store and organize data

## The Collections Framework
- provides a set of interfaces (`List`, `Set`, `Map`) and set of classes (`ArrayList`, `HashSet`, `HashMap`, etc.) that implement those interfaces.
- these are parts of `java.util` package

## Java List - Interface
- represents an ordered collection of elements
- access by their index, add duplicates, and maintain the insertion order
- `List` is an interface
### Common List Methods
- `add()` - add an element to the end of the list
- `get()` - returns the element at the specified position
- `set()` - replaces the element at the specified position
- `remove()` - removes the element at the specified position
- `size()` - returns the number of elements in the list
### List vs. Array
| Array                             | List                              |
| --------------------------------- | --------------------------------- |
| fixed size                        | dynamic size                      |
| faster performance for raw data   | more flexible and feature-rich    |
| not part of collections framework | part of the collections framework |
### `ArrayList`
- a resizable array

| built-in array                                                                                                                 | `ArrayList`                                                             |
| ------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| the size of an array cannot be modified. If you want to add or remove elements to/from an array, you have to create a new one) | elements can be added and removed from an `ArrayList` whenever you want |
#### Create an `ArrayList`
```java
import java.util.ArrayList; // Import the ArrayList class

ArrayList<String> cars = new ArrayList<String>(); // Create an ArrayList object
```
- `.clear()` - to remove all elements in the `ArrayList`

⚠️ Elements in an `ArrayList` are actually objects. To use `ArrayList` of non-primitive type, must specify an equivalent ***wrapper class***
- `int` -> `Integer`
- `boolean` -> `Boolean`
- `char` -> `Character`
- `double` -> `Double`

#### Sort an `ArrayList`
- Another useful class in the `java.util` package is the `Collection` class, which include the `sort()` method for sorting lists alphabetically and numerically
```java
Collections.sort(<name_of_the_List>, Collection.reverseOrder() <- optional);
```

#### The List interface
⚠️ Sometimes you will see both `list` and `ArrayList`:
```java
import java.util.List;
import java.util.ArrayList;

List<String> cars = new ArrayList<>();
```
- This gives more flexibility to change the type later
```java
List<String> cars = new LinkedList<>();
```

### `LinkedList()`
- Same same but different on how each works
#### How the `ArrayList` works
- `ArrayList` class has a regular array inside it
- When an element is added, it is placed into the array
- If the array is not big enough, a new, larger array is created to replace the old one

#### How the `LinkedList()` works
- stores its elements in "containers"
- the list has a link to the first container (head) and each container has a link to the next container in the list

> When to Use what
> `Array List` - store and access data
> `LinkedList` - manipulate data

#### `LinkedList()` methods
- `addFirst()` - adds an element to the beginning of the list
- `addLast()` - adds an element to the end of the list
- `removeFirst()` - remove an element from the beginning of the list
- `removeLast()` - remove an element from the end of the list
- `getFirst()` - get the element at the beginning of the list
- `getLast()` - get the element at the end of the list

## Java Set
- used to store collection of **unique elements**
- does not preserve the order of elements (unless using `TreeSet` or `LinkedHashSet`)
#### Common Set Methods
- `add()` - adds an element if it's not already in the set
- `remove()` - removes element from the set
- `contains()` - checks if the set contains the element
- `size()` - returns the number of elements
- `clear()` - removes all elements

### Java HashSet
#### Create a `HashSet`
```java
import java.util.HashSet; // Import the HashSet class

HashSet<String> cars = new HashSet<String>();
```

### `TreeSet`
- a collection that stores unique elements **in sorted order**
#### Create a `TreeSet`
```java
import java.util.TreeSet; // Import the TreeSet class

TreeSet<String> cars = new TreeSet<>();
```

⚠️`TreeSet` is slower than `HashSet` in terms of performance due to sorting

### `LinkedHashSet`
- stores **unique elements** and **remembers the order they were added**
#### Create a `LinkedHashSet`
```java
import java.util.LinkedHashSet; // Import the LinkedHashSet class

LinkedHashSet<String> cars = new LinkedHashSet<>();
```

⚠️`LinkedHashSet` is slightly slower than `HashSet` due to order tracking

## Map
- used to store key-value pairs
- Each key must be unique, but values can be duplicated
### Common Map Methods
- `put()` - adds or updates a key-value pair
- `get()` - returns value for a given key
- `remove()` - removes the key and its value
- `containsKey()` - check if the map contains the key
- `keySet()` - returns a set of all keys

### `HashMap`
- stores items in **key/value pairs**, where each map to a specific value.
- instead of accessing elements by an index, use a key to retrieve its associated value
#### Create a `HashMap`
```java
import java.util.HashMap; // Import the HashMap class

HashMap<String, String> capitalCities = new HashMap<>();
```

### Add key-value pairs to hash map
```java
HashMap<String, String> capitalCities = new HashMap<String, String>();
capitalCities.put("England", "London");
```
⚠️ If an entry with the same key already exists then the newly added pair overrides the old one
#### Loop through a `HashMap`
- use for-each loop
- use the `keySet()` method if you only want the keys, and use the `value()` method if you only want the values
```java
// Print keys
for (String i : capitalCities.keySet()) {
  System.out.println(i);
}
```

```java
// Print values
for (String i : capitalCities.values()) {
  System.out.println(i);
}
```

### `TreeMap`
- a collection of that stores key/value pairs in **sorted order by key**

#### Create a `TreeMap`
```java
import java.util.TreeMap; // Import the TreeMap class

TreeMap<String, String> capitalCities = new TreeMap<>();
```

⚠️ `TreeMap` is slower than `HashMap` as it maintains sorted order

### `LinkedHashMap`
- stores keys and values, and keeps them in the same order you put them in

#### Create a `LinkedHashMap`

```java
import java.util.LinkedHashMap; // Import the LinkedHashMap class

LinkedHashMap<String, String> capitalCities = new LinkedHashMap<>();
```

## `Iterator`
- an object that can be used to loop through collections
#### Getting an Iterator
```java
// Get the iterator
    Iterator<String> it = cars.iterator();
```

#### Looping through a Collection
```java
while(it.hasNext()) {
  System.out.println(it.next());
}
```

#### Removing Items from a Collection
- Iterators are designed to easily change the collections that they loop through.
- The `remove()` method can remove items from a collection while looping.
```java
ArrayList<Integer> numbers = new ArrayList<Integer>();
    numbers.add(12);
    numbers.add(8);
    numbers.add(2);
    numbers.add(23);
    Iterator<Integer> it = numbers.iterator();
    while(it.hasNext()) {
      Integer i = it.next();
      if(i < 10) {
        it.remove();
      }
    }
```
