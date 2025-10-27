- Each thread class necessarily has to have a `run()` method which is invoked wheng `thread_object.start()`
	- Analogy: `pthread_create()` in POSIX Standard
- Runnable:
	- Instead of extending Thread, you can implemetn the Runnable interface

## Executor System
- high-level API for managing threads
- decouples task submission from thread managament 
- supports thread pools and task scheduling
	- Thread pool: set of threads that can be assigned by the executor

```java
// fixed-size thread pool
ExecutorService fixedPool = Executors.newFixedThreadPool(4);

// Cached thread pool (free size)
ExecutorService freePool = Executors.newCachedThreadPool();

for (int i = 0; i < 10; ++i) {
	fixedPoo.submit(() -> doWork());
	// OR
	cachedPool.submit(() -> doWork());
}
cachedPool.shutdown();
```

## `synchronized` keyword
- Java provides its own locking mechanisms for objects
	- 1 lock - 1 object
- non-static methdos can be declared synchronized


## Semaphores