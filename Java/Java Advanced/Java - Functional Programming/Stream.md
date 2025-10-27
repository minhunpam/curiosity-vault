## Definitions
- _stream_ = a sequence of data 
- _stream pipeline_ consists of the operation that run on a stream to produce a result
- _Lazy Evaluation_ #Jargon  = data isn't just generated up front - it is created when needed, which delays execution until necessary

## Understanding the Pipeline Flow
- There are 3 parts to a stream pipeline:
	1. **Source**: where the stream comes from
	2. **Intermediate Operations**: transforms the stream into another one. There can be as few or as many intermediate operations as you'd like. 
		- Since streams use lazy evaluation, the intermediate operations do not run until the terminal operation runs
	3. **Terminal Operation**: actually produces a result. Since streams can be used only once, the stream is no longer valid after a terminal operation completes

![[Java - stream_pipeline_illustration.png]]

## Creating a Stream Sources
### Creating Finite Streams

#### Converting a `Collection` to a stream using `stream()`
```java
var list = List.of("a", "b", "c");
Stream<String> fromList = list.stream();
```


### Creating Infinite Streams



## Common Terminal Operations
### `forEach()`
- iterate over the elements of a stream

Method signature:
```java
void forEach(Consumer< ? super T> action)
```




## Common Intermediate Operations
### `map()`
- creates a one-to-one mapping from the elements in the stream to the elements of the next step in the stream

Method signature:
```java
<R> Stream<R> map(Function<?, super T, ? extends R> mapper)
```

##### Example: converts a list of `String` objects to a list of `Integer` objects representing their length;
```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
s.map(String::length)
	.forEach(System.out::print); // 676
```

