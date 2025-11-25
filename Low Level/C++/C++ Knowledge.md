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


