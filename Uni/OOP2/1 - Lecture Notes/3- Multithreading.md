## Parallelism and Distribution
### Parallelism (Concurrency)
- Typically refers to a given (one) computer
- Multitasking of applications
- True parallelism (multiple CPUs and/ or cores)
- Pseudo parallelsm (thread/ process switching)
- Threads (and processes) provide parallelism for improving resource sharing
### Distributed system
- Networked components, coordinated by messages
--> Server processes handle different types of access (e.g: web, mail, file)
--> Server threads within server process handle concurrent user requests

## Clients and Server with Threads
- **Typical client side**: User interface, processing logic, network communication
- **Typical server side**: Multiple clients, mulitple communications

## Java threads
- JVM runs as a process on the operating system
- A program is started by launching a new thread which runs the main method
- Run in parallel (on multiple cores/ CPUs) or in pseudo-parallel (by switching)
- Pauses thread for approx. the specified amount of time
- Exact time when thread will be resumed is difficult to predict
- Should not be used to synchronize threads
```java
class MyThread extends Thread {
	public void run() {
		System.out.println("Hello from thread!");
		Thread.sleep(1000);
		System.out.println("Huh, who is this?");
		Thread.sleep(1000);
	}
}

MyThread t = new MyThread();
t.start();
```

### `Runable`
- Instead of extending `Thread` you can implement the `Runnable` interface
- Alternative to using run on objects dervied from `Thread`
- Applicable for code in objects not derived from `Thread` (__note: no multiple inheritance in Java__)
```java
class MyRunnable implements Runnable {
	public void run() {
		System.out.println("Hello from Runnable!");
	}
}

Thread t = new Thread(new MyRunnable());
t.start();
```
- Since Java 8, can use a lambda expression for simple `Runnable` implementations
- Useful for short, inline thread logic
```java
Thread t = new Thread(() -> {
	System.out.println("Hello from lambda Runnable!");
})
t.start();
```

## Java Thread States
![[OOP2 - State Diagram.png]]
- A new thread object is created using `start()` to enter the active states (`run()`)
- There are 2 active states:
	1. **RUNNABLE**
		- The thread is **ready to run**
		- It is **eligible for CPU**, but may not be executing right now
		- Waiting in the scheduler’s queue
	2. **RUNNING**
		- The thread is **currently executing** on the CPU
		- Its `run()` method is actively being executed
	- Transitions between them: Scheduler decides who runs:
		- `yield()` or preemption: **RUNNING -> RUNNABLE**
			- `yield()`is when a running thread voluntarily gives up the CPU
			- preemption: is when the scheduler forcilby takes the CPU away from a running thread
				- The thread does not choose
				- Controlled by the OS/ JVM scheduler
				- Happens automatically
		- Being scheduled: **RUNNABLE -> RUNNING**
- State **WAITING**:
	- The thread voluntarily waits
	- It will not run until another thread wakes it up
	- Relevant functions:
		- `wait()`
		- `nofify()`, `notifyAll()`
- State **BLOCKED**:
	- The thread cannot continue because it is waiting for something external
	- Typical causes:
		- Waiting for **I/O** to finish
		- Waiting for a **lock / monitor**
		- `sleep()`
		- Blocking I/O operations
- State **TERMINATED**:
	- The thread has finished execution
## Thread Managment
### Executor System
- High-level API for managing threads
- Decouples task submission from thread managament 
- Supports thread pools and task scheduling
	> A thread pool is a fixed group of reusable worker threads that execute tasks for you
	> Instead of creating a new thread for every x
```java
// Fixed thread pool example
ExecutorService fixedPool = Executors.newFixedThreadPool(4);
for (int i = 0; i < 10; ++i) {
	fixedPool.submit(() -> doWork());
}
fixedPool.shutdown();
```
- Create a thread pool with exactly 4 worked threads (at most 4 tasks run in parallel)
- Wraps the lambda into a task and place the task into the **task queue**
	- We submitted 10 tasks
	- The first 4 tasks were picked up by the 4 threads - The remaining 6 tasks "waited" in the queue to be executed
	- When a thread finshes a task, **it immediately takes the next task from the queue**
- `shutdown()` after all tasks have been finished --> kills threads immediately
```java
// Cached thread pool example
Executor cachedPool = Executors.newCachedThreadPool();
for (int i = 0; i < 10; ++i) {
	cachedPool.submit(() -> doWork());
}
cachedPool.shutdown();
```
- Cached pool has no fixed size
- **It creates and removes threads dynamically**
- From the above example, the cached pool could create 10 threads if the work takes some time and there are no idle threads available
```java
BlockingQueue<Runnable> queue = new LinkedBlockingQueue<>();
ExecutorService queuePool = new ThreadPoolExecutor(
	2, // core pool size
	8, // maximum pool size
	60, // idle thread keep-alive time
	TimeUnit.SECONDS,
	queue
);

for (int i = 0; i < 10; ++i) {
	queuePool.execute(() -> doWork());
}
queuePool.shutdonw();
```
- The blocking queue is somewhat the combination of **fixed thread pool** and **cached thread pool**, because:
	- It can create and kill threads dynamically
	- It has a finite size range (**upper bound is not unlimited like cached thread pool**)

#### Automatic resource allocation
- Avoid hard-coding pool size - **Use the platform paralleism**
```java
int procs = Runtime.getRuntime().availableProcessors();
ExecutorsService fixed = Executors.newFixedThreadPool(procs);
```

- `Executors.newWorkStealingPool()` pick a parallelism based on `Runtime.availableProcessors()`
```java
ExecutorsService pool = Executors.newWorkStealingPool();
for (int i = 0; i < 50; i++) {
	pool.submit(() -> cpuBoundTask());
}
```
1. A task is submitted
2. It goes into **one worker’s local queue**
3. Worker processes tasks from the **front**
4. If worker runs out of work:
    - It steals a task from the **back** of another worker’s queue
5. This balances the load automatically
> **`newWorkStealingPool()` is ideal for CPU-bound tasks as it keeps all cores of the CPU busy, which avoids busy-waiting. But for blocking I/O tasks which wait for something external and keeps all threads/cores block on I/O. It would lead to a drop in CPU usage to ~0% and throughput collapses. That's why for block I/O, a larger/ separate pool is prefered.**

## Synchronization
- Threads often work on shared data
- The execution order of threads may be hard to predict
- Untrolled access may lead to inconsistents
- Synchronization methods are need to control the data access, for:
	- Provision of data consitency (maintaining data correctness)
	- Maintaining parellelism (maintaining runtime performance)
	- How to trade-off these may depend on the level of data contesting
### `synchronized` keyword
- Java provides its own locking mechanisms for objects
	- 1 lock - 1 object
- non-static methods can be declared `synchronized`
- If a thread calls synchronized method, the runtime environment will attempt to lock the object for the thread
	- If the object already locked, requesting thread is put to sleep (efficient blocking)

```java
class Bank {
	public synchronized void transferMoney(int accountNumber, float amount) {
		account[accoutNumber].transferMoney(amount);
	}
}
```
- Because the method is declared as `synchronized`, Java implicitly does: `synchronized(this)`. So the lock is on the Bank object itself <=> **Only 1 thread can execute any synchronized method of this Bank instance at a time**
- Scenario:
	-  Thread T1: transfer to account #1
	- Thread T2: transfer to account #2
Logically, these could run in parallel, because they operate on different accounts
- Result:
	- T1 enters `transferMoney`
	- T2 must wait
	- **Only one transfer at a time** --> No parallelism
> **Attention to locking scope**. In the code snippet above, the entire bank is locked, instead of a specific account

Fix: 
```java
class Bank {
    public void transferMoney(int accountNumber, float amount) {
        Account acc = account[accountNumber];
        synchronized (acc) {
            acc.transferMoney(amount);
        }
    }
}
```

### Semaphores
- A semaphore is a synchronization aid that controls access to a shared resource
- Semaphores maintain a set of permits; acquire permits before accessing resources
- Useful for limiting the number of concurrent accesses a resource (e.g. connection pools)
```java
import java.util.concurrent.Semaphore;

Semaphore sem = new Semaphore();
sem.aquire();
try {
	// access shared resource	
} finally {
	sem.release();
}
```