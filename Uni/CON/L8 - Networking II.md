## IPv6
- Internet Protocol, version 6
- Successor to IPv4
- Not natively interoperable with IPv4
	- IPv4-only devices cannot communicate with IPv6-only devices
	- Most modern devices implement both IPv4 and IPv6
	- Eventually, IPv4 will be phased out
- 128-bit address
	- Notation: 16-bit hexadecimal blocks separated by colons (`:`)
	- Zero blocks can be omitted using double colon (`::`)
		- `fe80::10e5::f700:f6ab:7afc` == `fe80:0000:0000:0000:10e5:f700:f6:ab:7a:fc`
		> **Note: you can use a double colon only once in a single address**

### IPv6 addressing
- 64-bit network prefix, 64-bit interface identifier
- A single interface (e.g.: a network card) may have multiple addresses
	- Addresses share the interface identifier
	- Example:
		![[CON - example of having multiple addresses for single interface.png]]
- Addresses have a scope in which they are valid

#### IPv6 Scoping
1. **Global addresses**
	- Valid in any network connected to the Internet
	- May be routed on the public Internet
2. **Unique-local addresses (in `fc00::/7)**)
	- Same idea as IPv4 private networks
		- No assignment/ registration needed
		- Routed only in local network, but not on the public internet
3. **Link-local addresses (in `fe80::/64**)
	- Link Layer Network

### IPv6 Packet Overview
- Similar fields to IPv4 packets
	- Version is always `0110` (version 6)
	- Length, Source and Destination fields
	- Optional extension header blocks
- Header checksum removed
	- Relies on Link Layer to provide error detection
- Fragmentation (mostly) removed
	- No fragmentation by routers
	- Fragmentation by hosts only as an extension
![[CON - IPv6 Packet Structure.png]]

## Transport Layer
- The internet has 2 widely-used protocols at the Transport Layer
	- **Transmission Control Protocol**
		- Focused on reliable delivery
		- Connection-based
	- **User Diagram Protocol**
		- Focused on speed
		- Connectionless

### Ports
- Concept used for both TCP and UDP
- Source and destination identified by port number
	- 16-bits (65536 available ports)
	- TCP and UDP ports are separate
		- The protocols implement the same idea, but each cares about its own ports
- Common notation: Port number after IP address
	- `127.0.0.1:8000` is port `8000` at host `127.0.0.1`
	- `[::1]:8000` is port `8000` at host `::1`
- **Two applications can't use the same port number**
- Client needs to know which port number to connect to
	- **0 - 1223: well-know ports**
		- Examples:
			- 22 (SSH)
			- 80 (HTTP)
			- 123 (NTP)
			- 194 (IRC)
			- 444 (HTTPS)
	- **1024 - 49151: registered ports**
		- Most server applications will use this range (even unregistered ones...)
	- **49152 - 65355: dynamic ports**
		- Most OS will use this range for ephemeral (client) ports

### UDP
- Fire-and-Forget transmission of single datagrams
	-  Useful for real-time applications
- Data may never arrive, may arrive out of order, ...
	- Data loss  must be tolerable for the upper-layer application
- Extremely simple and straightforward

![[CON - UDP datagram header.png]]

## TCP
- Highly reliable transmission of a byte stream
	-  Acknowledgements and re-transmission
	- Guaranteed to maintain data ordering
- Non-trivial protocol overhead
	- Still better than re-inventing the wheel if you need it
- TCP connections have 2 sides: _server_ and _client_
	- Server listens on a specific port
		- Server port is fixed for all connections
	- Client connects to that port on the server
		- Client uses a "random" ephemeral port, different for each connection
		- Check: `netstat -tnap`
	> Connections are uniquely identified by **Client IP + Client Port**

### TCP Packet Header
- Source + Destination ports allow identification of connection
- Checksum over entire header + data
- TCP maintains a sequence number across the entire connection
	- Separate number for each end's packets
- Receipt of contiguous data confirmed via _Acknowledgement Number_ (if ACK set)
	- Acknowledgement number := next expected sequence number
- This allows ordering of data and re-sending of lost packets
- Connection establishment: Three-way handshake
	- Client --> Server: **SYN** 
		- Sequence Number: `seq_c`, chosen randomly
	- Server -> Client: **SYN + ACK**
		- Sequence Number: `seq_c`, chosen randomly
		- Acknowledgement: `seq_c + 1`
	- Client -> Server: **ACK**
		- Sequence Number: `seq_c + 1`
		- Acknowledgement: `seq_c + 1`
- Not both sides know that other has their sequence number --> Ready to communicate!

### TCP Flow Control
- Context: _A supercomputer sending data over a 100Gbps link to a desk phone_ — the phone cannot keep up and would crash if data arrived too fast
- The flow control mechanism tells the sender the maximum speed at which the data can be sent to the receiver device.
	- The sender adjusts the speed as per the receiver's capacity to reduce the frame loss from the receiver side
	- Flow control ensures that it'll not send more data to the receiver in the case when the receiver buffer is already completely filled
#### Sliding Window Protocol
- The receiver sends the receiver window which indicates the currently available in the receiver's buffer, to the sender
- **From the available receiver window, TCP calculates how much data can be sent further without acknowledgment**
	> If the receiver window size is zero, TCP halts the data transmission until it becomes a non-zero value
### TCP Congestion Control
![[CON - TCP Congeston Control.png]]
- Congestion occurs when the rate of flow of packets towards any node is more than its handling capacity ---> slows down the network response time ---> The performance of the network decreases
- There are three phases that TCP uses for congestion control: 
	1. Slow start
	2. Congestion avoidance
	3. Congestion detection
- Basic concept:
	- Start at relatively slow rate, then increase speed until data gets lost
	- Once the data is lost, assume we overloaded the connection and slow down again

#### `cwnd` == Congestion Window
- TCP state variable that limits how much unacknowledged data a TCP sender is allowed to have "in flight" (**= sent but not yet ACKed**) at anytime

### TCP Selective Acknowledgement (SACK)
- When a packet loss is detected in TCP communication between client and server, the next step is to recover the lost data packet. **SACK** is one of the algorithms to do that

#### How SACK works
- SACK helps the sender to identify ‘gaps’ in the receiver buffer
- When the receiver does not receive packets in order then there exist some holes(missing points) in the buffer --> SACK helps the sender to know about these holes in the receiver buffer at an early time
- When the sender knows about the holes/ lost packets --> it retransmits them at the same time

![[CON - SACK example.png]]
- In this example, the reason for the duplication of `ACK-1` or `SACK 2-3` amongst packets 0,2,4 is because:
	- TCP is **event-drivent,** meaning for every packet received, the receiver immediately sends a corresponding ACK. The duplication of `ACK-1` only means "I still haven't received the next expected pakcet"
	- The `SACK` blocks **accumulate knowledge** of what the receiver has:
		- packet-4 arrives --> `SACK 4-5`
		- packet-5 arrives --> `SACK 4-6`
		--> the `SACK` block expands with every newly coming 
#### When to use `ACK`(plain ACK number) or `SACK`(selective acknowledgement)
- `ACK` - always reports the next expected in-order byte/packet --> It tells what is the lowest missing packet
- `SACK` - eports **additional packets that arrived _out-of-order_** --> tells which later packets were received even though gaps remain




