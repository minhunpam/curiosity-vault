## Program, thread, process
### Program: a binary file containing code and data
- actions: write, compile, install, load
- resources: file
- mold for a process

### **Thread**: sequence of things (instructions) that happen, or an execution context
- actions: run, interrupt, stop
- resources: time on the CPU, stack, registers
	- thread ID
	- thread state
	- a set of register values
	- stack
- if part of a process -> restricted to the boundaries of a process
- share the same address space (because of the same process)
- **keywords: dispatcher thread, worker thread**
	
### **Process**: a container for threads and memory contents of a program
- actions: create, start, terminate
- resources: threads, memory, program
- an instance of a program
- is created at boot time
- processes don't run, but live dependently on the lifetime of threads. If there are no threads left, the process loses its meaning -> die

>> ⚠️ Do not confuse ***shared memory(between processes)*** with ***shared address space (between threads)***

#### How process terminates
1. Normal termination (caused by `exit()`)
2. Error termination
3. Fatal error
4. Termination by another process

## `pid_t fork(void)`
- SHALL create a new process (_child process_) that SHALL be **an exact copy** of the calling process (_parent process_) except:
	1. child has its own unique process ID
	2. The child process SHALL be created with a single thread. f a multi-threaded process calls `fork()`, the new process shall contain a replica of the calling thread and its entire address space, possibly including the states of mutexes and other resources

- The child process is the exact copy of the parent's memory, registers, file descriptors,etc. .But it doesn't mean the child starts from the very beginning of the program, it starts right where the parent left off: after the `fork()` call

- child does not know the parent
- parent knows the child
- `waitpid` - parent waits for child to die
## Return Value
- SHALL return 0 to the child process and SHALL return the process ID of the child process to the parent process
- Otherwise, -1 SHALL be returned to the parent process, no child process SHALL be created


## exec
### execvpe
- replace running process with process defined by `file`


## `pthread_create(pthread_t* thread, const pthread_attr_t* attr, void* (*start_routine) (void*), void* arg)` 
- `thread` = pointer to a thread variable where the system can store the ID of the new thread
- `attr` = pointer to a 'thread attributes' object that defines thread properties (default attributes: NULL/0 **most of the time using this option is fine**)
- `routine` = function pointer that the thread will execute. 
	- SHALL return `void*` and accept a `void*` argument
- `arg` = a single argument passed to the thread function. 
	- `NULL/0` if no argument is needed
	- can pass a struct or pointer to pass multiple values

- starts a new thread in the calling process. The new thread starts execution by invoking `start_rountine()`; `arg` is passed as the sole argument of `start_rountine()`

### start_routine can only take none or single argument, but what if you want to pass > 1 arguments
- Solution: Use the wrapper function as the start routine
```c
void wrapper_function(size* xyarray) {
	
}

void 


```

### `int pthread_join(pthread_t thread, void** retval)`
- waits for the thread specified by `thread` to terminate. If that thread has already terminated, then `pthread_join()` returns immediately
- There are 3 possible values that `void** retval` can have:
	1. If the thread’s `start function` returned something with `return ...;` → that pointer value is stored into `retval`.
	2. Thread calls `pthread_exit(void* value)`, then that same `value` is stored into `retval`
	3. Thread was cancelled, then `retval` will be set to the constant `PTHREAD_CANCELED`

## `void pthread_exit(void* retval)`
- terminates the calling thread and returns a value via `retval` that (if the thread is joinable) is available to another thread in the same process that calls `pthread_join()`
- After the last thread in a process terminates, the process terminates as calling `exit()` with an exit status of zero

## `int pthread_cancel(pthread_t thread)`
- sends a cancellation request to the `thread`

## How do `pthreads` terminate?
- It calls `pthread_exit()` -> specify value that will be returned to any thread calling `pthread_join()` on it.
- Any of the threads in the process calls `exit()`, or the main thread performs a return from `main()` -> termination of all threads in the process
- It is cancelled (`pthread_cancel()`) 

