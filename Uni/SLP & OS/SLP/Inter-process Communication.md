## `exec` family of functions 


| Part | Meaning                                                     |
| ---- | ----------------------------------------------------------- |
| `-l` | "list" - arguments are passed individually (comma-separted) |
| `-v` | "vector" -  arguments are passed as array(`char* argv[])`   |
| `-p` | "path" - searched `PATH` for executable                     |
| `-e` | "environment" - lets you pass your own `envp[]` array       |

## When does the shared memory object actually take up physical RAM?
## `shm_open()`
- The kernel creates metadata entr  in the virtual filesystem

## `ftruncate()`
- This sets the size of the shared memory object, but still:
	- The kernel only records that it should have that size
	- **No physical memory is allocated yet**

## `mmap()`
- maps virtual address space into your process -> tells kernel: "These virtual pages are backed by that shared object"
- when a process writes to a page in that mapping, the CPU triggers a page fault, then the kernel allocates a real RAM page and maps it in

## Clean-up Process of shared memory
### `munmap(addr, length)`
- Unmaps the region from your process's virtual address space
- After `munmap`, the memory region (`addr` ... `addr + length`) is no longer accessible by your process
- It does not
	- delete the underlying shared memory object
	- affect other processes that have mapped the same region

### `close(fd)`
- Close your process's **file descrptor** for the shared memory object
- Your process no long has a handle to the shared memory object
- The shared memory object itself still exists until all references are gone and it's unlinked

### `shm_unlink(name)`
- removes the name of the shared memory object from the global namespace
- After `shm_unlink`, no new processes can `shm_open(name)` it
- The shared memory object itself will actually be destroyed only after
	- all processes had it open before close their file descriptors
	- it has been unlinked

## How to implement mutex + condition variable between processes
```C
typedef struct {
	pthread_mutex_t mutex;
	pthread_cond_t cond;
	int shared_resource;
} SharedMemory;

int fd = shm_open(SHM_NAME, O_CREAT | O_RDWR, 0666);
ftruncate(fd, sizeof(SharedData));
SharedData *shared = mmap(NULL, sizeof(SharedData), PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
```
1. Initializize attributes
```C
pthread_mutexattr_t m_attr;
pthread_condattr_t c_attr;

pthread_mutexattr_init(&m_attr);
pthread_condattr_init(&c_attr);

pthread_mutexattr_setpshared(&m_attr, PTHREAD_PROCESS_SHARED);
pthread_mutexattr_setpshared(&c_attr, PTHREAD_PROCESS_SHARED);
```
2. Initialize mutex and cond in shared memory
```c
pthread_mutex_init(&shared->mutex, &m_attr);
pthread_cond_init(&shared->cond, &c_attr);
shared->shared_resources = 0;
```
3. Clean up attributes (not needed after init)
```C
pthread_mutexattr_destroy(&m_attr);
pthread_condattr_destroy(&c_attr);
```

