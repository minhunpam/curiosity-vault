## Reactive Manifesto
### Development of reactive software
- Need for reactive programming nowadays:
	- Big data
	- Heterogenenous environment from small devices to server farms
	- User expectations: fast reponses in milliseconds, always available
- Processing done in parallel
- Stream-orientation
- Asynchronous processing
- Respond to data (and events, failures, messages) rather than loading all data and processing it completely before coming back with a result
- Coupling of heterogenous applications/ components

### Concept
![[OOP2 - reactive manifesto.png]]
- **Responsive**: the system responds in a timely manner: cornerstone of usability
- **Elastic**: System stays responsive under varying workloads; react to changes in input load, adapting resource usage
- **Resilient**: The system stays responsive in case of failure
- **Message driven**: Asynchronous Message passing establishes boundaries and loose coupling

## Indirect communication
- Basis for distributed systems, interoperability, parallel and reactive applications, redundancy . . .
- Temporal Uncoupling
	- Asynchronous communication
	- Sender and receiver do not need to be active (or even exist) at the same time
	- Thread-based tasks!
- Space Uncoupling
	- Identity of sender or receiver does not need to be known to other side
	- Allow exchange, update, or addition of recipients
### Transient communication
- **Transient**: A message exists **only temporarily**. If it cannot be delivered immediately, it is **lost**.
#### 1. Transient asynchronous
- **Key idea:**  
	- The sender sends a message and does **not wait**, but the message is delivered **only if the receiver is running at that moment**.
- **How it works:**
	- Process **A** sends a message to process **B**.
	- **A continues immediately** after sending (non-blocking).
	- The system does **not store** the message.
	- If **B is running**, it receives the message.
	- If **B is not running**, the message is **lost**.

#### 2. Receiver-oriented transient 
- **Key idea:**  
	- The sender sends a request and **waits until the receiver acknowledges receipt**, but the receiver does not need to process the request immediately.
- **How it works:**
	- Process **A** sends a request to **B**.
	- **A blocks (waits)** until it receives an **ACK** (acknowledgment).
	- **B must be running** to receive the message.
	- **B immediately sends an ACK** to confirm receipt.
	- The actual processing of the request can happen **later**.
	- The message is **not stored permanently** (still transient).

### Persistent communication
- **Persistent**: A message is **stored** until it can be delivered, even if sender or receiver is not running.
#### 1. Persistent asynchronous (store on sender side)
- **Key idea:**  
	- The sender sends a message and continues immediately. The system **stores the message** until the receiver becomes available.
- **How it works:**
	- Process **A** sends a message to **B**.
	- **A does not wait** (asynchronous).
	- The message is **stored on the sender side** (or in a sender-side queue).
	- **B may be not running** at the time of sending.
	- When **B starts running**, it receives the stored message.
	- Even if **A stops running** after sending, the message is still delivered.

#### 2. Persistent synchrnous (store on receiver side)
- **Key idea:**  
	- The sender sends a message and **waits until the receiver accepts it**, but the receiver does **not need to be running at that moment**.
- **How it works:**
	- Process **A** sends a message to **B**.
	- The message is **stored at B’s side** (receiver mailbox or queue).
	- **A blocks** until it receives confirmation that the message has been **accepted and stored**.
	- **B may not be running** when the message arrives.
	- When **B starts running**, it retrieves and processes the stored message.
	- After acceptance, **A may stop running** safely.

### Push vs. Pull notifications
Both **push and pull** can be:
- **Reliable** → delivery guaranteed (ACKs, retries)
- **Unreliable** → best-effort (may lose messages)
#### 1. Pull Notification:
- **The receiver asks for information.**  Nothing is sent unless someone explicitly requests it.
- How it works
	- Client sends a **request**: “Do you have new data?”
	- Server replies **only after** the request.
	- Client repeats this periodically (**polling**).

#### 2. Push Notification:
- Distributed systems often need updates on local object changes (events)
- Useful to connect heterogeneous systems
- Efficient notification via multicast
- Registering of callbacks (reactive programming)

### Applications
- Indirect communication used for distributed systems in which ...
	- Changes are happening frequently
		- Nodes connect and disconnect frequently
	- Nodes are unknown (anonymous nodes)
- Examples
	- Mobile environments
	- Peer-to-peer communication

### Space and Time Coupling in Distributed Systems
- **Time-coupled**:
	- Sender and receiver must be **active at the same time**.
- **Time-uncoupled**:
	- Sender and receiver do **not** need to be active at the same time.
- **Space-coupled**:
	- The sender **knows exactly who the receiver is** (identity or address).
- **Space-uncoupled**: The sender **does not know** the identity of the receiver(s).

|                      | Time-Coupled                                                                                                                     | Time-Uncoupled                                                                                                                       |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Space-coupling**   | - Sender targets a specific receiver<br>- Receiver(s) must be running now<br><br>**Example: Message passing, remote invocation** | - Sender targets a specific receiver<br>- Receiver can be offline<br>- Message is stored for later delivery<br><br>**Example: Mail** |
| **Space-uncoupling** | - Sender does not know receiver identity<br>- Receiver must be active now<br><br>**Example: IP multicast**                       | - Sender does not know receiver<br>- Receiver can be offline<br>- Most flexible and scalable<br><br>**Example: Message queues**      |
### Group communication
- Example (network): IP multicast
	- Infrastructure support to deliver IP packets to multiple destinations
- Example (application): Mailing list
	- List server expands to list names
- Commonalities
	- Clients: need operations to join, leave groups
	- Multicast vs broadcast

### Message Queues
A **message queue** is an **intermediate system** that stores messages sent by **producers** and delivers them to **consumers**.
![[OOP2 - Message queue.png]]

- **Producers (left)**
	- Applications that **create and send messages**
	- They do **not send messages directly** to consumers
	- They only know the **queue**, not who consumes the message
- **Message Queue System (middle)**
	- Holds one or more **queues**
	- Stores messages **persistently**
	- Manages delivery, ordering, retries, acknowledgments
- **Consumers (right)**
	- Applications that **process messages**
	- They retrieve messages from the queue
	- Multiple consumers can exist
- Valid behaviors:
	- **Receive**
		- Consumer explicitly fetches a message
		- Often blocks until a message arrives
	- **Notify (Push)**
		- Queue notifies consumer when a message is available
		- Event-driven
		- Low latency
	-  **Poll (Pull)**
		- Consumer repeatedly asks:  “Do you have a message?”
		- Simpler but may waste resources

- Examples:
	- RabbitMQ
	- MQTT: IoT applications
	- Apache Kafka

### Publish-Subscribe
- **Publishers** send messages to **topics**
- **Subscribers** express interest in topics
- A **Pub/Sub system (broker)** delivers messages to all interested subscribers
![[OOP2 - Publish-Subscribe.png]]
- **Publishers (left)**
	- Produce messages (events, data)
	- Send messages to **topics** (e.g. `t1`, `t2`)
	- Do **not know** who will receive the message
- **Publish–Subscribe System (middle)**
	- Acts as a **broker**
	- Keeps track of:
	    - Topics
	    - Subscriptions
	- Forwards messages to all matching subscribers
	- Subscribers (right)
	- Register interest using **SUBSCRIBE**
- **Subscribers (right)**
	- Register interest using **SUBSCRIBE**
	- Can subscribe to:
	    - A specific topic (`t1`)
	    - Multiple topics (`t*` using patterns)
	- Can later **UNSUBSCRIBE**
#### Delivery Guarantees
- **At most once**: not resilient, usually ephemeral messages
- **At least once**: resilient, but might double deliver messages
- **Exactly once**: guarantees exactly one delivery of each messge
#### Publish-Subsribe
- By Channel: e.g. name
- By Topic: flag on channel
- By Type: super-topic
- By Content: Based on structure of content/ attributes

## Stream Processing
- Streams provide a high-level abstraction for processing sequences of elements
- Can be processed sequentially or in parallel
- Functional-style operations (map, filter, reduce, etc.)

```java
List<Float> numbers = FloatStream.rangeClosed(1, 100_000);
numbers.stream().map(Math::exp).forEach(Systen.out::println);
```

```java
List<Float> numbers = FloatStream.rangeClosed(1, 100_000);
numbers.stream()
	.parallel()
	.map(Math::exp)
	.forEach(System.out::println);
```

## Virtual threads
- Java 21 introduces virtual threads for lightweight concurrency
- Virtual creates an executor that assigns each task to a new virtual thread
- Great for high-concurrency workloads: blocking I/O threads!

## Asynchronous Programming
- Traditionally, programs used **blocking I/O**:
	- When a process sends a request (network, file, disk, etc.)
	- It **waits (blocks)** until the operation finishes
	- During that time, the thread does **nothing useful**
	---> **This wastes CPU time and reduces responsiveness**
- **Asynchronous programming** allows a process to:
	- Start a long-running operation
	- **Continue executing other work**
	- Handle the result **later**, when it is ready
- Java uses `Future` and `CompletableFuture` for async tasks
- Submit tasks to an executor and retrieve results later
```java
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(() -> {
	Thread.sleep(1000);
	return 42;
});
System.out.println("Doing other work...");
Integer result = future.get();
System.out.println("Result: " + result);
executor.shutdown();
```

