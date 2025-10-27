## Does the `return` in lambda function #CppQuestions 
If really depends on the use cases
- `return` is required in cases if the code using it expects a value (such as `std::transform` )
- Skip `return` if only an action is need, such as print in `std::for_each`

## Steps to implement for copy constructor #CppQuestions 
- For primitive data type, copy like usual through initializer list
- For pointers or dynamically allocated memory, allocate new memory and copy the content

## Virtual Destructor #CppKnowledge #CppQuestions 
- ensure the correct destructor of a derived class is called when you delete an object when you delete an object through a base class pointer
- Without it, **only the base class destructor** will be executed --> resource leaks/ undefined behavior
```
class Base {
public:
    virtual ~Base() {
        cout << "Base destructor called" << endl;
    }
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "Derived destructor called" << endl;
    }
};

int main() {
    Base* obj = new Derived();
    delete obj; // Calls both Derived and Base destructors
    return 0;
}
```
Output:
```
Derived destructor called
Base destructor called
```

### Use case:
- **Always** in base classes meant for polymorphism (base class with at least one virtual function)