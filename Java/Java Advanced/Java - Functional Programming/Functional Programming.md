## Consumer interface #Java8 #FunctionalProgramming 
- is a **built-in functional interface** in `java.util.function`
- **Type parameter** `T` → the type of input it takes
- **Abstract method** → `accept(T t)` → performs an operation on `t` and **returns nothing** (`void`)

[[Functional Interfaces]]

## `Optional`
- `Optional` expresses "we don't know" or "not applicable"
- You can either request an empty `Optional` or pass a value for the `Optional` to wrap. Think of an `Optional` as a box that might have something in it or might instead be empty
```java
public static Optional<Double> average(int... scores) {
	if (scores.length == 0) return Optional.empty();
	int sum = 0;
	for (int score : scores) sum += score;
	return Optional.of(sum/scores.length);
}
```

### Checking the emptiness of an `Optional` value
- Not empty:
```java
Optional<Double> opt = average(90, 100);
if (opt.isPresent()) System.out.println(opt.get());
```
- Empty:
```java
Optional<Double> opt = average();
System.out.println(opt.get()); // NoSuchElementException
```
- `NoSuchElementException` == no value present

### `ofNullable(value)`
- If `value` is **non-null** → returns an `Optional` wrapping that value.
- If `value` is **null** → returns an **empty Optional** (instead of throwing `NullPointerException`)

#### Difference with `.of()`
- `Optional.of(value)` → throws `NullPointerException` if `value` is `null`
- `Optional.ofNullable(value)` → safely handles `null` by returning `Optional.empty()`

```java
Optional.of(null);        // ❌ NullPointerException
Optional.ofNullable(null); // ✅ Optional.empty()
```


