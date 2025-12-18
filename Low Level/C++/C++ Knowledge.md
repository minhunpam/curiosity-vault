## `final` keyword next the class name #CppKnowledge
- Prevent further inheritance or overriding
```
class Base final {
    // ...
};

// This is NOT allowed and will cause a compile error:
class Derived : public Base { }; // Error: Base is final
```

[[Design Patterns]]

## `explicit` keyword before class constructor #CppKnowledge 
- prevents implicit conversions or copy-initialization using a constructor that takes a single argument
- If you don't mark a single-argument constructor as `explicit`, C++ may silently perform automatic type conversions --> unexpected behavior or bugs

### Example without `explicit`
```
class MyClass {
public:
    MyClass(int x) {
        // constructor
    }
};

void print(MyClass obj) {
    // ...
}

int main() {
    print(42);  // ✅ This works because 42 is implicitly converted to MyClass
}
```

### Use case:
- For constructors with one parameter

## `namespace`
- **name-scoping mechanism**
### Why do we use namespaces
#### 1. Avoid name collisions across files
- If multiple files that have the same function signature. Without namespace, you get linker errors
```cpp
namespace snp {
	void init();
}

namespace ui {
	void init();
}
```
- We we call the function:
```cpp
snp::init();
ui::init();
```

## `new` and `delete`
### `new`
- If enough memory is available, it initializes the memory with a default value based on its type and returns the address of the allocated memory
```cpp
type* pointer_name = new type(size);
```
- Allocate block of memory (array)
```cpp
type* pointer_name = new[number_of_elements];
// We can specify the elements
type* pointer_name = new[number_of_elements]{e1, e2, ... , eN};
```

#### What if enough memory is not available during runtime?
- If enough memory is not available in the heap to allocate, the new request indicates failure by throwing an exception of type `std::bad_alloc` (unless `nothrow` is used with the new operator, in which case it returns a `nullptr` pointer)

### `delete`
- `delete` operator is used to release dynamically allocated memory. It deallocates memory that was previously allocated with `new`
```cpp
delete ptr;
```
- To free a dynamically allocated array pointed by pointer available
```cpp
delete[] ptr;
```




