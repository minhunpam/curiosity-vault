- atomic = cannot be interrupted

## Locking mechanism
- fairness -> use a queue for threads to wait after a thread terminates
- atomic swapping operation:
> The value of memory location is swapped with a new value, and the old value is returned, all in a single CPU instructions
### Spinlock #Multithreading
- "Spinning" means repeatedly checking (**busy-wait loop**) whether **the lock is free - without yielding the CPU or sleeping**
```c
size_t lock(size_t* lock) {
 while (*lock == 1) { // while it is lockec
	 //busy wait
 }
 // Get out the loop when it is unlocked
 // And lock
 *lock = 1;
 return 0;
}
```
- But the problem with this code is that it is not ATOMIC!!!
- The reason: Assume there are 2 threads trying to acquire the lock
	- Thread A checks `lock == 0 (lock is free)`
	- Thread B checks `lock == 0 (lock is free)(still true)`
	- Both threads set `lock == 1 (unlock) -> race condition`

```c
int pthread_spin_lock(pthread_spinlock_t* lock);
int pthread_spin_unlock(pthread_spinlock_t* lock);
```

> Spinlocks are not efficient
> Instead of busy waiting
> 	 put thread to ***sleep***
> 	 keep a ***list of sleeping threads***
> 	 wake up a sleeping thread when unlocking

### Mutex #Multithreading 
```c
int pthread_mutex_lock(pthread_mutex_t* mutex);
int pthread_mutex_unlock(pthread_mutext_t* mutex)
```
- Only the thread that locks the mutex can unlock it
- used for locking a resource to ensure that only one thread accesses it a  time

### Condition Variable #Multithreading
- Synchronization mechanism (synchronization primitive) that lets threads wait for a condition to become true while effectively sleeping, and lets other threads signal when that condition may have been changed

- Not inherently thread-safe:
	- Using mutex to make it thread-safe
- Three main operations:
1. `pthread_cond_wait(pthread_cond_t* cond, pthread_mutex_t* mutex)` - wait for an event
2. `pthread_signal` - wake up 1 waiting thread
3. `pthread_broadcast` - wake up ALL waiting threads

```C
pthread_mutex_t mutex_fuel;
pthread_cond_t cond_fuel;

// Shared resource

int fuel = 0;
  
void* fuel_filling(void* arg) {
	while (1) {
		pthread_mutex_lock(&mutex_fuel);
		fuel += 15;
		printf("Filling fuel... Current fuel: %d\n", fuel);
		pthread_mutex_unlock(&mutex_fuel);
		// pthread_cond_signal(&cond_fuel);
		pthread_cond_broadcast(&cond_fuel); // wake up all waiting threads
		sleep(1);
	}
return NULL;
}

// ?: Does it make any difference (unlock, then signal) from (signal, then unlock)?

  

void* car_combusting(void* arg) {
	pthread_mutex_lock(&mutex_fuel);
	while (fuel < 40) {
		printf("No fuel. Waiting...\n");
		pthread_cond_wait(&cond_fuel, &mutex_fuel);
		// pthread_cond_wait is equivalent to:
		// pthread_mutex_unlock(&mutex_fuel);
		// wait for signal... on cond_fuel -> sleep
		//....
		// wake up and relock the mutex
		// pthread_mutex_lock(&mutex_fuel);
	}
	fuel -= 40;
	pthread_mutex_unlock(&mutex_fuel);
	sleep(1);

return NULL;
}
```

#### `pthread_cond_signal()`
- wakes up 1 thread (if any and randomly) waiting on the given condition variable
- does not release any mutex
- does not wait for the woken thread to run

#### Why is `pthread_cond_wait` usually placed within a `while` loop, but not an `if` statement? #Synchronization_pitfalls
1. ***Spurious Wakeups***
	- threads can be woken up even if **no signal** or **broadcast** was sent
	- If using `if`, the thread would wrongly assume the condition is satisfied and continue execution  -> causing lots of problems related to shared resource
	- A `while` ensures it goes back to waiting if the condition is still `false`
2. Multiple wating threads
	 - If several threads are waiting on the same condition variable, a single `pthread_cond_broadcast()` wakes up all of that thread, but only 1 of them can take the mutex
	 - If using `if`, they'd all continue 
	 - If using `while`, they'll recheck and wait again

#### Why shouldn't we unlock mutex first then signal condition variable? #Synchronization_pitfalls 
- Short answer: The waiting thread might ***miss the signal*** entirely if it checks the conditino before you signal and then goes to sleep afterward

| T1 (waiter)                                                                       | T2 (signaller)                            |
| --------------------------------------------------------------------------------- | ----------------------------------------- |
| checks `ready == false`                                                           |                                           |
| before actually calling `pthread_cond_wait`, the OS suspends it                   | context switch to T2                      |
|                                                                                   | signal happens                            |
|                                                                                   | no thread is wating yet -> signal is lost |
| resume                                                                            |                                           |
| call `pthread_cond_wait` and goes to sleep, but no one will ever wake it up again |                                           |
**Result: Deadlock (the waiter sleeps forever)**

Now if signal happens before the unlock, there will be 2 scenarios:
- If T1 goes first, an OS suspension could happen, but no signal is placed outside of the mutex, then T1 will just wait until the suspension is over then it continues
	- `pthread_cond_wait` - sleeps and releases the mutex
	- T2 takes it signal it -> unlock 
	- T1 is woken up and continues working
- If T2 goes first - lock the mutex -> change the shared resource -> signal (but there is no waiting thread - nothing happens) -> unlock mutex 
	- T1 takes the mutex -> check the condition -> `true` -> not enter the while loop -> unlock the mutex

⚠️ In either way, ***no signal is lost***

## Semaphore
- mechanism used multithreaded programs for managing the access to shared resources (memory/ files)

> A POSIX semaphores can be **named** or **unnamed**

- `int sem_wait(sem_t* sem)` - lock or wait a semaphore
- `int sem_post(sem_t* sem)` - release or signal a semaphore
- `sem_init(sem_t* sem, int pshared, unsigned int value)`
	- `sem` - specifies the semaphore to be initialized
	- `pshared` - specifies whether or not the newly initialized semaphore is shared between processes/ between threads
		- A non-zero value == the semaphore is shared between processes
		- A value of zero == shared between threads, in the same process
	- `value` - specifies the value to assign to the new initialized semaphore
- `sem_destroy(sem_t* sem)`
	



#### Flow of a Consumer-Producer Example:
- **Consumer**:
	- checks the condition under the mutex
		- if not satisfied -> calls `pthread_cond_wait`
			- releases the mutex and puts the thread to sleep in condition variable's "waiting room"
			- later, when signaled, it wakes up and re-acquires mutex (`pthread_mutex_lock()`)
- **Producer**:
	- changes the shared state/ resource
	- calls `pthread_cond_signal` (or `pthread_cond_broadcast`) -> signal sleeping thread(s) to check the condition

## Practice
- Determine the resource which can be accessed by threads
- Which of them are interconnected
- one resource, one lock