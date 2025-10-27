- technique to solve problems that involve ***sub-array/ sub-string/ window***
- maintain a range ("window") that moves step-by-step through the data, updating results incrementally
- main idea: use the results of the previous window to do computations for the next window
- commonly used for problems:
	- finding sub-arrays with a specific sum
	- finding the longest sub-string with unique characters
	- that require a fixed-size window to process elements efficiently

## 2 types of sliding window:
### 1. Fixed Size Sliding Window:
- Find the size of the window required, say K.
- Compute the result for 1st window, i.e. include the first K elements of the data structure.
- Then use a loop to slide the window by 1 and keep computing the result window by window.

### 2. Variable Size Sliding Window:
- In this type of sliding window problem, we increase our right pointer one by one till our condition is true.
- At any step if our condition does not match, we shrink the size of our window by increasing left pointer.
- Again, when our condition satisfies, we start increasing the right pointer and follow step 1.
- We follow these steps until we reach to the end of the array.