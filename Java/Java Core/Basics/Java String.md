- One important characteristic of a String: ***immutable*** <- achieved through the use of a special [[#String Constant Pool]]
## String Constant Pool
- a separate place in heap memory where all the strings defined in the program are stored
- When we declare a string:
	- an object of type `String` is created in the stack
	- its value is stored in the heap in the string constant pool.

* one way to skip this memory allocation is use the `new keyword` while creating a new string object.
* the new keyword forces a new instance to always be created regardless of whether the same value was used previously or not

## Convert 