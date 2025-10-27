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
