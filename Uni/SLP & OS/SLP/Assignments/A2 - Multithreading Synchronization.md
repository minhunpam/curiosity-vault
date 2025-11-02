## Framework:
### Roles:
- ***Racers***: 
```C
typedef struct {
	size_t id;
	size_t tid;
	Leaderboard* lb;
} Racer;
```
- do a practice lap:

- do "hotlap" (Race day):
	- 
- ***Engineers***:
```C
typedef struct{
	size_t id;
	size_t tid;
	bool available;
	bool can_go_home;
	Racer* assigned_racer;
} RaceEngineer;
```
- _waiting for_ racers to assist them with their lap

## Shared Resources:
1. Waiting queue for racers in practice session
2. green flag
3. available engineers
4. safety crew

## Practice Session:
### Enqueueing and Dequeuein
- Create its own mutex `queue_mutex`
	- Racer: lock -> enqueue waiting racer -> unlock
	- Practice engineer: lock -> check if is empty
		- `true` -> take the first waiting racer of the queue -> dequeue -> unlock
		- `false` -> unlock -> print out the break message + take a break
	
### Leaderboard and Time assignment
- create RACERS-array of mutexes
- create RACERS-sized array of condition variables
	- for leaderboard creation
	- for time assignment
> Or we can create RACER-array of condition variables for both tasks
> As long as for each pair of racers and engineers, they have their own mutex, own condition variable(s), then it is fine.

The reason why we have to do that is, racer-engineer-pairs would work concurrently, so if we keep only 1 mutex, 1 condition variable for all pairs, the concurrency is basically broken

##### Flow: for 1 racer-engineer-pair

| Racer                                                                                               | Engineer                             |
| --------------------------------------------------------------------------------------------------- | ------------------------------------ |
| lock mutex                                                                                          |                                      |
| check the condition and it is false                                                                 |                                      |
| wait (release the mutex)                                                                            | take the mutex                       |
|                                                                                                     | lock mutex                           |
|                                                                                                     | create leaderboard                   |
|                                                                                                     | signal the condition +  unlock mutex |
| wake up + lock the mutex where he slept                                                             | giveInstruction                      |
| checkRacerBeforePracticeSession                                                                     |                                      |
| racer has leaderboard                                                                               |                                      |
| unlock mutex                                                                                        |                                      |
| drivingLap                                                                                          |                                      |
| lock mutex                                                                                          |                                      |
| check the condition and it is false                                                                 |                                      |
| wait (release the mutex)                                                                            | take mutex                           |
|                                                                                                     | lock mutex                           |
|                                                                                                     | assign time                          |
|                                                                                                     | signal the condtion + unlock mutex   |
| wake up + lock mutex where he slept                                                                 |                                      |
| checkRacerAfterPracticeSession                                                                      |                                      |
| racer has been assigned with time                                                                   |                                      |
| free resource (this could happen before or after the unlock, because it is not the shared resource) |                                      |

## Hot lap
### Green flag
- There 1 green flag (shared resource), if a race day engineer signals the flag, all racers should be able to find their engineers
- Create a `queue_mutex`
	- racer(s): wait on the condition variable at the same time
	- engineer: broadcast all sleeping threads
### Available engineers (Searching for available engineers)
- mutex: `available_engineers_mutex`
- semaphores: 
	- `sem_racer` - default value: 0 - 1 sem for all racers
		- because it is 0 initially, racer will just wait
		- The reason why not creating sems for each racer is because later on when engineers `sem_post`, at the moment he hasn't had info about racer's id
		- So when the initial value is 0, all threads could be blocked there and when `sem_post`, racer will be "picked" randomly
	- an array of `sem_engineers` - default value: 1 - each engineer has his own sem

| Racer                                                                                                                | Engineer                                             |
| -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| sem_wait(&sem_racer) , current 0, blocked                                                                            | `sem_wait(&sem_engineers[race_engineer->id])` 1 -> 0 |
|                                                                                                                      | lock mutex                                           |
|                                                                                                                      | available to true                                    |
|                                                                                                                      | post racer sem                                       |
|                                                                                                                      | unlock                                               |
| one of the racers is posted + lock mutex                                                                             |                                                      |
| findAvailableEngineers() cannot return Null, because there has been available engineer there -> no possible segfault |                                                      |
| assign himself to the race engineer                                                                                  |                                                      |
| unlock mutex                                                                                                         |                                                      |
| `sem_post(&sem_engineers[race_engineer->id])`                                                                        | 0 -> 1                                               |
|                                                                                                                      | checkEngineerBeforeRace                              |
|                                                                                                                      | available to false                                   |
|                                                                                                                      | unlock mutex                                         |

```C
vector_iterator engineer_it = findAvailableRaceEngineer();
RaceEngineer* engineer_it = (RaceEngineer*)*engineer_it;
```
- what if there is no available engineer (meaning: `engineer_it == NULL`), dereferencing it will lead to Segfault
	- This possiblity comes from a situation where the number of racers > the number of engineers

1. Problem: Racers and Engineers are waiting at the same time
![[Pasted image 20251018122732.png]]
## `can_go_home`
```C
if (race_engineer->can_go_home) {
	printf("RACE ENGINEER %zd is going home now!!\n", race_engineer->id);
	pthread_mutex_unlock(&available_engineers_mutex);
	break;
}
```
- Does it make any difference when switching `prinf` and `pthread_mutex_unlock`
- Holding the log inside the critical section at `formula_student.c:314` means the thread still owns `available_engineers_mutex` while `printf` runs
	- `printf` can block briefly (stdio locking, flushing to the terminal), so any other threads trying to lock `available_engineer_mutex` sit and wait -> delay
	- If unlocking before the log, the mutex become availables immediately and other threads can move on and free to change shared state
	- If the log message only prints immutable data like `race_engineer->id` -> safte to log after unlocking
	- If you need to print fields that might change, copy them into locals before unlocking

1. There is a problem I am thinking about where to signal and turn `race_engineer->can_go_home = true` in the racer thread when we have more racers than engineers
	- Let's 

### Leaderboard creation and Time assignment
- Same idea as in pratice session

#### Free Resources
- Because the engineer is responsible for resourcing allocated memory, but there could be a case that the engineer finishes before the racer and free an allocated resource which will be dereferenced racer -> Seg-fault
- In approach, I introduced an array of semaphores set to 0 by default, each for an engineer and an array of mutexes
	- But the use of mutexes in this case is not necessary
- Racer (after time assignment): sem_post for engineer
- Engineer: free the resource

### Engineers continue with the next racer
- In this part, we have to use the same mutex `available_engineers_mutex` again, because it holds the share resource `available engineer`
- In this session, the engineer actively makes himself available again so it would make sense to skip the first `sem_wait` in the "Searching for available engineers" by adding a `sem_post`
- By skipping the first `sem_wait`,  we will move to the second `sem_wait` , that's where the beauty of semaphore really shines
	- Because `sem_wait` is a cancellation point
	- After all racer's theads have been cancelled, it then comes to the termination of engineers
	- `pthread_cancel` will send cancellation request to engineers' threads which are waiting for coming racers
	- The engineer threads that are wating at the `sem_wait` will terminate immediately

### Engineers take a break
- shared resource: `available_engineers` -> use the mutex: `available_engineer_mutex`

### Racers take drug test
- The part seemed trivial at the first place, when I only introduced a semaphore, but the race condition comes when let's say 2 racers choosen for taking drug test try to take the same slot of safety crew. That was when I introduces another mutex to make sure no race condition could occur