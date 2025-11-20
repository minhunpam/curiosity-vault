## Overview
- Airport communication sytem
	- one tower process (initial process)
		- creating and owning the shared memory objects of the airport
		- also spawns all service and plane processes
	- multiple concurrent service/ plane processes

### Airport Structure
- Airport:
	- Gate:
		- gate number
		- gate status
		- plane index
	- Runway:
		- status
		- current plane on the runway \[ID\]
		- next plane to take off \[ID\] - `queue_front`
		- next plane to land \[ID\] - `queue_front`
- Tower: 
	- controller of the live action an airport + always has the final say about all movements around the airport
	- Sole participant to activate the frequency
		- Planes/ Services MUST wait until `!FREQ_INACTIVE` -> interact with the frequency
- Plane: 
	- **Each plane is a separate process**
	- Actions:
		- Land
		- Take off
	- Attributes:
		- direction
		- flight number
		- number of passengers
		- next plane index (if in a queue), otherwise: `UNASSIGNED`
- Service:
	- Small utility vehicles providing various services to planes
	- **Each plane is serviced once**
	- Attributes:
		- vehicle number
		- status
		- servicing plane id

### Radio Transceiver
- Radio Transceiver is a radio client device with 2 actions:
	- Transmitter
	- Receiver
- Each radio participant has its own local instance of a radio transceiver
	- with own local variables and **local message queue**
	- Tower
	- Service vehicles : 0 -> `NUMBER_SERVICES`
	- Planes: 0 -> `NUMBER_PLANES_TO_SPAWN`

```C
typedef struct _RadioTransceiver {
	RadioParticipant ident_;
	Envelope last_received_envelope_;
	pthread_t transmit_tid_;
	Envelope* queue;
	
	
} RadioTransceiver;
```
#### Radio Participant
```C
typedef struct _RadioParticipant {
	RadioParticipantKind kind_;
	size_t index_;
} RadioParticipant;
```

#### Envelope
```C
typedef struct _Envelope {
	RadioParticipant transmitter_;
	RandioParticipant receiver_;
	Message message_;
	struct_ Envelope* next_;
}
```
- Data Structure for envelop: Linked List/ The of the message queue
- `struct_ Envelope* next_`
	- pointer to the next envelope when the envelope is in a local transceiver queue
	- **If it is not in a queue, reading/ writing from this value is undefined behavior**
	
##### Message
```C
typedef struct _Message {
	MessageKind kind_;
	union {
		size_t data_;
		struct {
			MessageKind instruction_kind_;
			size_t instruction_data_;
		} readback_;
	};
} Message;
```



## Shared Memory between Process & Shared Resources between threads
### Shared Memory Between Processes
- `RadioFrequency shm_tower_freq_t`
```c
typedef _RadioFrequency {
	RadioStatus status_;
	Envelope envelope_;
} RadioFrequency;
```
- Q: When tower sets the frequency to `FREQ_IDLE` and envelope to `NOENVELOPE`


\- `shm_airport_t`
	- `Runway runways_[NUM_RUNWAYS]`
	- `Gate gates_[NUM_GATES]`
	- `size_t registered_planes_[NUM_PLANES_TO_SPAWN]`
		- 1 -> registerd
		- 0 -> not registered
	- `size_t plane_wanting_gate_queue_`
	- `size_t plane_wanting_service_queue_`
- `filedescriptors fds`
	- initialized with all `-1` in `airport.c`
```c
typedef struct _filedescriptors
{
	int fd_shm_tower_freq_;
	int fd_shm_airport_;
	int fd_shm_services_;
	int fd_shm_planes_;
	int fd_shm_locks_;
} filedescriptors;
```
- `mappings mmaps`
	-  initialized with all `NULL` in `airport.c`
```C
typedef struct _mappings
{
	shm_tower_freq_t* tower_freq_;
	shm_airport_t* airport_;
	shm_services_t* services_;
	shm_planes_t* planes_;
	shm_locks_t* locks_;
} mappings;
```
- `shm_planes_t`
	- contains `Plane vehicles_[NUM_PLANES_TO_SPAWN]` -> susceptible for RACE CONDITION!

### Shared Resources
- `plane_pids[NUM_PLANES_TO_SPAWN]` 
	- array to store plane PIDs
	- initialized with zero designated initializer in `airport.c`
- `service_pids[NUMBER_SERVICES]`
	- array to store service vehicle PIDs
	- initialized with zero designated initializer in `airport.c`
- `pthread_t spawn_planes_thread`
	- initialized in `tower.h`
	- use for `spawnPlaneAsync(void)`
		- What is really the point of this?
-
## Todos:
### `tower.c`
- `int startTower(int argc, char* argv)`
	- only allowed to add synchronization if needed
- `void initAirportSHM(void)`
	- init all shared objects for the airport and resize them
	- if a segment is not needed, set its fd to -1
- `void initLocks(void)`
	- Initialize all synchronization primitives here
	- Use `mmaps.locks_`
- `void spawnServiceVehicles(void)`
	- spawn a service vehicle process here
	- a service process expects its index as the first argument
		- use an `exec()` function
		- `startService(int argc, char* argv[])`
		- use `(void)idxStr`
			- This does nothing basically, only stop warnings from compiler
	- set the service pid in the `service_pids`
- `void spawnPlane(size_t plane_idx)`
	- spawn a new plan process here
	- pass the plane index as an argument to the plane process (possibly an `exec` function)
		- use `(void)plan_idx_str`
	- in parent process, set the plan PID in the `plane_pids` array accordingly
- `void waitForSpawnedProcesses(void)`
	- wait for all spawned child processes (services, planes) here
	- should the operation fail, exit the process with `WAITPID_FAILED` (declared in `common.h`)
- `void cleanupAirportResources(void)`
	- clean up all resources of the airport here (including locks)
	- after cleanups, make sure not to leave dangling fds/mmap pointers
	- reset fds to -1 and mmap pointers to NULL after cleanup
### `plane.c`
- `int startPlane(int argc, char* argv[])`
	- main communication loop for a plane
	- are only allowed to add synchronization (locking, conditions, etc.) if need
- `initPlaneSHM(void)`
	- initialize all shared memory segments for the plane and resize them
	- If segment is not needed, set its fd to -1
	- use th `SHM` constants and `MODERW`, defined in `common.h`
	- Use `fds` declared in `airport.c`
- `initPlaneMappings(void)`
	- initialize all mappings for the plane
	- if a segment is not needed -> set its pointer to `NULL`
	- use the variable `mmaps` declared in `airport.c`
- `PlanDirection initPlaneState(int plane_idx)`
	- Initialize the plane state in the mapping with the prepared local plane state
		- Question: Do we need to lock here? Because in `spawnPlane(size_t plane_idx)`, each plane has already been assigned to unique id in the array so I think no need to lock it here -> I was right!
- `void cleanupPlaneResources(void)`
	- clean up all resources of the plane here
	- after cleanup, make sure not to leave dangling fds/mmap pointers
	- reset fds to -1 and mmap pointers to NULL after cleanup
### `service.c`
- `int startService(int argc, char& argc[])`
	- main communication loop for a service
	- only add synchronization (locking, conditions, etc.) if needed
- `initServiceSHM(void)`
	- initialize all shared memory segments for the service and resize them
	- If segment is not needed, set its fd to -1
	- use th `SHM` constants and `MODERW`, defined in `common.h`
	- Use `fds` declared in `airport.c`
- `void initServiceMappings(void)`
	- initialize all mappings for the service
	- if a segment is not needed -> set its pointer to `NULL`
	- use the variable `mmaps` declared in `airport.c`
- `cleanupServiceResources(void)`
	- clean up resources of the service here

### `transceiver.c`
#### Transceiver-local Synchronziation Primitives
- `void rt_init(RadioTransceiver* self, RadioParticipant* self_ident`)
	- init your transceiver-local synchronization primitives
- `void rt_destroy(RadioTransceiver* self)`
	- destroy the transceiver-local synchronization primitives
#### Shared Frequency
- Initialized:
	- `pthread_mutex_t shared_frequency_mutex`
	- `pthread_cond_t waiting_ready_frequency`
- `void rt_setFreqReady(RadioTransceiver* self)`
	- lock
	- set frequency status to `FRED_IDLE` and envelope to `NOENVELOPE`
	- broadcast
	- unlock
- `void rt_awaitFreqReady(RadioTransceiver* self)`
	- lock
	- while look check the status
		- true: wait until signal
	- unlock
#### Others
- `void rt_receive(RadioTransceiver* self, Envelope* envelope)`
	- waits until:
		- frequency is `FREQ_BUSY_MSG_PRESENT`
		- there is a message present with receiver matching the radio transceiver's `self_ident` in terms of:
			- kind
			- index
	- after receiving a message, ***THE FREQUENCY MUST NOT BE CLEARED***
	- transceiver's owner is guaranteed to call exactly one of the functions:
		- `rt_reply()`
		- `rt_replyReadBack()`
		- `rt_clearFreq()`

	- Implementation:
		- lock `locks.shared_frequency_mutex`
			- wait on frequency status and whether the receiver's info is matching transceiver's owner's info
			- take the envelope
##### Message queue (shared resource between the main thread and single transmit thread of the same process)
- shared mutex: `rt.message_queue_mutex`
- shared cond: `rt.waiting_message_queue_fininsh_actions`

- `void rt_enqueMessage(RadioTransceiver* self, RadioParticipant receiver, Message message)` <- **called in the main thread of the process**
	- `void rt_enqueue(RadioTransceiver* self, Envelop* envelope)`
		- This function **MUST NOT** touch the frequency directly
		- Sending MUST be done via the `_rt_transmitThread`
	
		- may be called at any time between
			- (`rt_setFreqReady()` and `rt_destroy()`) or (`rt_awaitFreqReady())` and `rt_destroy()`) --> guarantee that the transceiver's owner does not call `rt_destroy()` _unless no enqueued messages are present_
		- ***MUST NOT BLOCK THE CALLING THREAD***
			- How to enqueue and send messages once the frequency is clear (frequency status -> `FREQ_BUSY_MSG_PRESENT` again
		HINT: `RadioTransceiver.queue` and `Envelope.next

		- Implementation:
			- lock around `rt_enqueueMessage` in process
			- null check the current node
				- if `NULL` -> set the envelope as queue head
				- else -> continue until the node is null them repeat previous step
			- signal
			- unlock
			- Question: 
				- Need lock here?
					- because each process (tower, plane, service) has its own radio transceiver <=> own message queue -> no other processes could interrupt that concurrently
		

- `void _rt_transmitThread(RadioTransceiver* self)` ***<- called in the async transmit thread of the same process***
	- send messages on the local queue one-by-one once the frequency is `FREQ_IDLE`
		- ***Question: What does this really mean?***
	- this function runs async to the radion participant's communication loop thread
		- can block it and wait for conditions to be met

	- Implementation:
		- lock `self->message_queue_mutex`
			- wait on `self->queue == NULL`
			- take the first element of the queue -> copy it -> assign to the temp
			- assign new queue head to the previous one's next
		- unlock

		- lock `locks->shared_frequency_mutex`
			- wait on the frequecy status `!= FREQ_IDLE`
			-  change status to busy and set frequency envelope
			- signal those waiting for the frequency status in `rt_receive()`
		- unlock

- `void rt_reply(Transceiver, Message message)`
	- Directly replies with a message to the transmitter of last received message

	- Implementation:
		- lock `locks->shared_frequency_mutex`
			- set frequency reply envelope
			- signal receiver
		- unlock

- `void rt_clearFreq(RadioTransceiver* self)`
	- directly clear the frequency by resetting its state to `FREQ_IDLE` and its envelope to `NOENVELOPE`


###  `common.h`
- `typedef struct { } shm_locks_t;`
	- add all shared synchornization primitives you need here
#### Note:
- There are 3 SHM flags
	- read-only | create -> not sure with use case yet
	- read-write | create
	- write-only | create -> not sure with use case yet

## Game of threads

| Tower 0                                                                                                                                                                                                                               | TransmitThread T0 | Plane 0                                                                                  | TransmitThread P0                                    | Plane 1                                                                                  | TransmitThread P1                                    |     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------- | --- |
|                                                                                                                                                                                                                                       |                   | enqueue `HELLO` to Tower 0                                                               |                                                      | enqueue `HELLO` to Tower 0                                                               |                                                      |     |
|                                                                                                                                                                                                                                       |                   |                                                                                          | decrement the `waiting_clear_freq_send_msg` (1 -> 0) |                                                                                          | wait on `waiting_clear_freq_send_msg`                |     |
|                                                                                                                                                                                                                                       |                   |                                                                                          | send the message back to Tower 0                     |                                                                                          |                                                      |     |
| - receive the message<br>- take the message details to reply                                                                                                                                                                          |                   | wait on `ready_receive_msg`                                                              |                                                      |                                                                                          |                                                      |     |
| go to HELLO case                                                                                                                                                                                                                      |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| reply<br>- broadcast `ready_receive_msg`<br>- start over the loop                                                                                                                                                                     |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| wait on `ready_receiver_msg`                                                                                                                                                                                                          |                   | go to HELLO case                                                                         |                                                      |                                                                                          |                                                      |     |
|                                                                                                                                                                                                                                       |                   | reply `REQUEST_LANDING`<br>- broadcast `ready_receive_msg`<br>- start over the loop      |                                                      |                                                                                          |                                                      |     |
| go to `REQUEST_LANDING` case                                                                                                                                                                                                          |                   | wait `ready_receiver_msg`                                                                |                                                      |                                                                                          |                                                      |     |
| - get registered plane id<br>- get runway id<br>- check if runway is clear<br>    - if true: assign plane to the runway<br>	- else:<br>	    - `pushPlaneTo` runway's landing queue (I don't think this would cause race conditon)<br> |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| 1: reply `CLEAR_TO_LAND`<br>0: reply `EXPECT_RUNWAY`                                                                                                                                                                                  |                   | go to `CLEAR_TO_LAND` case                                                               |                                                      |                                                                                          |                                                      |     |
| wait on `ready_receiver_msg`                                                                                                                                                                                                          |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| go to `ACKNOWLEDGE` case<br>- clear frequency -> any threads that wait on `waiting_clear_freq_send_msg`, in this case, that could be:<br>- plane 0<br>- plane 1 -> take this                                                          |                   | - get runway id<br>- replyReadBack<br>- sleep<br>- enqueue `LANDING_COMPLETE` to Tower 0 |                                                      |                                                                                          |                                                      |     |
| wait on `ready_receiver_msg`                                                                                                                                                                                                          |                   |                                                                                          |                                                      |                                                                                          | decrement the `waiting_clear_freq_send_msg` (1 -> 0) |     |
|                                                                                                                                                                                                                                       |                   |                                                                                          |                                                      |                                                                                          | send the message back to Tower 0                     |     |
| - receive the message<br>- take the message details to reply                                                                                                                                                                          |                   |                                                                                          |                                                      | wait on `ready_receive_msg`                                                              |                                                      |     |
| go to HELLO case                                                                                                                                                                                                                      |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| reply<br>- broadcast `ready_receive_msg`<br>- start over the loop                                                                                                                                                                     |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| wait on `ready_receiver_msg`                                                                                                                                                                                                          |                   |                                                                                          |                                                      | go to HELLO case                                                                         |                                                      |     |
|                                                                                                                                                                                                                                       |                   |                                                                                          |                                                      | reply `REQUEST_LANDING`<br>- broadcast `ready_receive_msg`<br>- start over the loop      |                                                      |     |
| go to `REQUEST_LANDING` case                                                                                                                                                                                                          |                   |                                                                                          |                                                      | wait `ready_receiver_msg`                                                                |                                                      |     |
| - get registered plane id<br>- get runway id<br>- check if runway is clear<br>    - if true: assign plane to the runway<br>	- else:<br>	    - `pushPlaneTo` runway's landing queue (I don't think this would cause race conditon)<br> |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| 1: reply `CLEAR_TO_LAND`<br>0: reply `EXPECT_RUNWAY`                                                                                                                                                                                  |                   |                                                                                          |                                                      | go to `CLEAR_TO_LAND` case                                                               |                                                      |     |
| wait on `ready_receiver_msg`                                                                                                                                                                                                          |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
| go to `ACKNOWLEDGE` case<br>- clear frequency -> any threads that wait on `waiting_clear_freq_send_msg`, in this case, that could be:<br>- plane 0 -> take this<br>- plane 1                                                          |                   |                                                                                          |                                                      | - get runway id<br>- replyReadBack<br>- sleep<br>- enqueue `LANDING_COMPLETE` to Tower 0 |                                                      |     |
| wait on `ready_receiver_msg`                                                                                                                                                                                                          |                   |                                                                                          |                                                      |                                                                                          |                                                      |     |
