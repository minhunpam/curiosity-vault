- Division of responsibilities
	- I don't care how my envelope gets there
	- The post office only needs to care about envelopes

- In networking, each layer has their own responsibilities and doesn't need to care what other layers do
- Modern internet uses the TCP/IP protocol suite

## The TCP/IP Model - has 4 layers
1. **Link Layer**
	- Send a chunk of data to a directly connected computer
![[CON - link layer.png]]
2. **Internet layer**
	- Route a chunk of data to a remote computer along a series of direct links
3. **Transport layer**
	- Transmit a structured bit stream  across the internet
4. **Applicaton layer**
	- Offer services without having to worry about details

## 1. Link layer
### Shared medium
- A network setup where **multiple devices use the same physical communication channel** to send and receive data
#### How it works
- All devices "listen" to the same medium. When one device sends data, all others can hear it. 
- Only one device can transmit at a time --> **If 2 devices send at the same time, the signals are garbled (confused)**
#### Key characteristics
- Collisions can occur (2 devices transmit at the same time) --> 
	- Use **CSMA/CA** for Wi-fi 
	- And **CSMA/CD** for Ethernet to avoid collsions
	![[CON - collision avoidance mean.png]]
	- However, **CSMA/CA** is imperfect due to **hidden node problem**
- Bandwidth is shared among all devices --> Performance degrades as more devices join

##### Hidden Node problem
![[CON - hidden node problem.png]]
- A listens and doesn't hear C --> conclude channel is idle --> transmit to B
- C listens and doen't hear A --> conclude channel is idle --> transmit to B
-  B receives 2 overlapping signals (from A and C) --> B gets garbled data --> frame(s) lost
### Example: Wifi (IEEE 802.11)
- Shared medium: wireless audio
	- If 2 nodes send at the same time, the signals are garbled
- No collision detection
	- If a node is sending, it cannot listten for transmissions at the same time
- Collision avoidance (CSMA/CA)
	- Imperfect due to the hidden node problem
	- Example:
- Central access point
	- Nodes communicate via the **Access Point (AP)**. What is means is node A wants to send data to Node B. Both are connected to the same Wi-Fi network. What actually happens:
	1. Node A sends the frame to the AP
	2. The AP forwards that frame to Node B
	> Even though A and B are in the same room, A never sends directly to B
- Data is acknowledged
	- Every successfully received data frame must be explicitly confirmed by the receiver with an ACK (acknowledgement) frame
	- How that works:
		1. Sender transmits a data frame
		2. Receiver checks the frame
		3. Receiver sends an ACK
		4. Sender receives the ACK
	- If there is no ACK, sender assumes **failure** --> retransmits the data

#### What does "IEEE 802.11" mean?
- **IEEE = Institute of Electrical and Electronics Engineers** - the International organization that develops networking standards
-  **802  = A family of standards for Local Area Networks (LANs)**
- **.11 = The specific working group for wireless LANs (WLANs)**
--> **IEEE 802.11 = the offical Wi-fi standard**

##### What **IEEE 802.11** defines?
- Specifies how Wi-fi works:
	1. **Physical Layer (PHY)**
		- Radio frequencies, Modulation schemes, Channel bandwidths, Data rates
	2. **Data Link Layer (MAC)**
		- CSMA/ CA, Frame formats, Acknowledgements (ACK), Retransmissions, Association with an AP

### LAN (Local Area Network) and Ethernet
- Refers to a small computer network that covers a limited area, such as:
	- Your home Wi-Fi network
	- A school computer lab
	- An office building network
	- A café Wi-Fi hotspot
	- A dormitory network floor
- Inside your LAN:
	- Devices talk directly
	- ARP, link-local IPv6, DHCP traffic all stay internally
	- Routers do NOT forward LAN-only traffic to the Internet

- Ehternet (IEEE 802.3) dominates as the **primary wired LAN standard**
> **It is a link-layer networking system that moves data between devices on the same LAN by sending structured Ethernet frames over a shared/ switched physical medium, identified by MAC addresses**

### Switched medium/ Ethernet switch
> **A switched medium is a network where each device has its own dedicated link to a switch, and the switch forwards frames only to the intended destination port.**

#### How it works
1. **Physical setup**
	- Each device plugs into its own switch port
	- Each link is point-to-point
2. **MAC learning (automatic)**
	- When a frame enters the switch --> Switch reads **source MAC** --> stores it in a table
3. **Frame forwarding**
	- When a frame arrives:
		1. Switch reads destination MAC
		2. Looks it up in the MAC table
			- If known -> forward frame only to that port
			- If unknown -> flood to all ports except the sender

### Addressing
- An address identifies a destination
	- Shared medium: recipients can recognize messages
	- Switched medium: we know where to forward messages to
#### Ethernet (IEEE 802.3) Frame Format

![[CON - Ethernet Frame Format.png]]
3. **Destination Address**: This is a 6-byte field that contains the MAC address of the machine for which data is destined
4. **Source Address**: This is a 6-byte field that contains the MAC address of the source machine
	- As source address (Unicast), the LSB of the first byte is always 0
5. **Length**: 2-Byte field indicates  the length of the entire Ethernet frame
	- This 16-bit field can hold a length between 0 and 65535, but length cannot be larger than 1500 Bytes
6. **Data**: This is the place where actual data is inserted, also known as **Payload**
7. **Cyclic Redundancy Check (CRC) == Checksum**:
	- 4 byte field
	- contains 32-bits hash code of data, which is generated over the Destination Address, Source Address, Length and Data fields
	- If the checksum computed by destination is not the same as sent checksum checksum value, data received is corrupted
	> **Checksums don't  help against an intelligent attacker**
8. **Ether Type Field:**: identifies the protocol carried in the payload of the frame
	- `0x0800` indicates that the payload is an IP packet
	- `0x0806` indicates that the payload is an ARP (Address Resolution Protocol) packet

#### Mac address
- **MAC = Media Access Control** - hardware numbers of a computer that are embedded into a network card (**Network Interface card**)
	- is also known as the Physical Address of a network device
- It identifies a network interface, not a user and not the whole computer
- 48 bits (6 Bytes), written in hexadecimal
```
	00:1A:2B:3C:4D:5E
```

#### Broadcast address: `FF:FF:FF:FF:FF:FF`
- Used in Ethernet and Wi-Fi
- All devices on the LAN accept the frame
- Used for **ARP requests** (used to find the MAC address that belongs to a known **IP address** on the local network) - "Who has this IP address?"

#### Ethernet: Switching
- Ethernet switches understand Link Layer Data
- Record source addresses to build map address <-> port
- Only forward  packets to the appropriate port
	- Minimize wasted bandwidth
	- No collisions possible

##### How it works?
- Computer A sends an Ehternet frame into the switch. The frame contains: **Source MAC** and **Destination MAC**. The switch reads the source MAC immediately and adds the source MAC and its port to the **MAC address table/ forwarding table**
- Because the frame's destination MAC is not yet in the table, the switch **floods the frame to all ports  except the incoming one**
- The destination computer (Computer B) replies and the reply frame has the same structure as the request frame. After that, the (MAC, port) pair of the destination is also added to the table
- Now everytime when computer A sends to computer B, the switch looks up in the table --> finds port --> forwards the frame only to that port
	✅ No other device receives the frame  
	✅ No wasted bandwidth  
	✅ No collisions
	- All future frames between these devices are directly switched (Flooding is no longer needed)
#### Ethernet: Switching Loops
- Multiple switches can be interconnected to form one big network
##### Problem: Broadcast Storm
- A broadcast is sent - Switch A floods it
	- Then A -> B, C; B -> C; C -> A, ... so on. The same broadcast frame keeps circulating forever
##### Solution
- **STP (Spanning Tree Protocol)**
	- Automatically disables redundant links until needed
	- Detects loops automatically
	- Builds a **loop-free logical tree**
	- **Disables (blocks) redundant links**
	- Keeps them as **backup**

#### Ethernet: V-LAN (Virtual LANs)
- A logical segmentation of a Layer 2 (Data Link Layer) network that enables devices to be grouped together regardless of their physical location
- Partition switch ports into different logical networks
- Devices on different networks cannot send packets to each other
- Broadcast packets are only broadcast to the device's VLAN

## 2. Network Layer
### IPv4 (Internet Protocol version 4)
- Is the most widely used system for identifying devices on a network
- separated by periods (`192.168.0.1`)
	- This address helps data find its way from one device to another over the internet

#### IPv4 Addressing
- consists of series of 4 8-byte numbers which are separated by decimal point (**dotted decimal notation**) which are known as **octets/bytes**
- The range of each octet is 0-255

##### Subnet Mask
- 32-bit number that divides an IP address into **network ID** and **host ID**
	- 1s represent the **network part** --> identify which network
	- 0s represent the **host portion** --> identify which device inside that network  
    --> Helps determine whether two devices are on the same local network or different ones.
![[CON - Role of Subnet Mask.png]]
 - Alternative notation: only specify the number of ones
	- `255.255.255.000` == `/24`
- All hosts with the same network prefix form a subnet
- Hosts within the same subnet can communicate directly, because **They're in the same Link Layer Network**
- Two addresses per subnet have special meaning:
	- ***Host number all zeros == NETWORK IDENTIFIER***
		- `10.27.152.142/24` is part of the `10.27.152.0/24` network
	- ***Host number all ones == BROADCAST ADDRESS***
		- `10.27.152.255/24`is the broadcast address for the `10.27.152.0/24` network
		- This address is used to send a packet to all devices in the subnet

- Subnet masks do not need to be full bytes. People often think subnet masks look like:
```
255.255.255.0   (/24)
255.255.0.0     (/16)
255.0.0.0       (/8)
```
- These masks stop exactily on byte boundaries (8 bits). Actually a subnet mask can stop at any bit, not just every 8 bits (**not full bytes**). For example:
```
255.255.255.240 (/28)
```
##### How many host addresses does a subnest have?
1. Count host bits
	- IPv4 has 32 total bits 
2. Compute total addresses
	- Each host bit can be 0 or 1 --> `total addresses = 2^{number of host bits}`
3. Subtract special addresses
	- Every subnet reserves:
		- **1 network address**
		- **1 broadcast address**
	- So usable hosts: `total addresses - 2`

Example: `192.168.13.80/28` has up 14 host addresses, because:
- `2^4 = 16` total addresses. This means: network addresses can only start at multiples of 16 in the last octet
	- So valid network addresses are (these are the **possible starting points for /28 networks**)
```
0, 16, 32, 48, 64, 80, 96, 112, ...
```

##### Not every broadcast address ends with `.255`
- What is the broadcast address of `192.168.195.0/28`?
	- Because this IP address belongs to `/28` subnet mask -> Each subnet has **16 addresses**, so if the starting address is `192.168.195.0` then the broadcast address is `192.168.195.15`
##### Not every address ending with `.255` is a broadcast address
- Take the network address `10.5.0.0\16`
	- That means: 
		- #(network bits) = 16
		- #(host bits) = 16
		--> So the broadcast address is `10.5.255.255` then `10.5.0.255` is just a normal host address

##### IP Configuration is done automatically
- In a home network, you usually have **an ISP modem/ router** that acts as a **DHCP (Dynamic Host Configuration Protocal) server** automatically assigning IP settings to your devices
	- A DHCP is protocol that automatically gives a device:
		- IP address
		- Subnet mask
		- Default gateway
		- DNS Server

##### How IPv4 (Layer 3) and the Data Link Layer (Layer 2) work together when you send data inside your local network
1. When your computer wants to send data, it first checks: "Is the destination IP in the same subnet as me?" by checking:
	```
	(destination IP) AND (subnet mask) ?== my network address
	```
	-  If the answer is Yes, the destination is **local (same LAN)**
2. If the destination is in the same subnet, the packet does not go to the router, it is sent directly over the local network (Ethernet/ Wi-Fi). This happens at the **Data Link Layer**, but we only know **the destination IP address**, whereas frames are deliverd at layer 2 using **MAC addresses**

- **So how do we get the MAC address?**
	--> **ARP (Address Resolution Protocol)**
	- Ethernet frames with type `0x0806` means: "This frame contains ARP"
	- When your device wants to talk to `192.168.1.25`
		1. It checks its **ARP cache**
		2. If missing --> send ARP request
			- Destination MAC: `FF:FF:FF:FF:FF:FF`(broadcast). This request is sent to every device on the LAN
		3. ARP reply (unicast):
			- Destination MAC: sender's MAC
			- Now the sender knows the MAC
	- Because broadcasts are expensive, which interrupts all devices and reduces performance. **ARP results are cached** --> This avoids repeating ARP for every packet

#### IPv4 Routing
- Routing happens when the destination address is not in the same subnet == the destination is not directly reachable
- It always starts with the question **"Is the destionation address in the same subnet?"**. If the answer is NO, the routing case kicks in
	- Every host and router has a routing table, whose entry says which destination network, which next hop to use
	- Take the destination IP --> look it up in the routing table --> choose the best matching rule --> get a next-hop IP
	- Uses ARP to get the MAC of the next hop --> wrap the IP packet in a frame --> send it to the next hop
- Each router moves the packet closer to the destination network, but only one step at a time 

#### Useful commands
- See your IP addresses: `ip addr` or `ifconfig`
- See your routing table: `ip route` or `netstat -rn`
- Watch a packet over the internet: `traceroute`

#### IPv4 Packet Overview
![[CON - ipv4 packet overview.png]]

- Version: always `0100`
- Twin "length" fields
	- Length of just the header (**4-bit**)
		- Optional header extensions may make it longer
		- **This field stores the number of 4-byte words, instead of bytes**
			- So for a 20-byte header, the header length would be: `20/4 = 5 words`
	- Length of this packet (**16-bit**)
- **Type of Service (8-bit)**: is used to indicate how a packet should be treated by the network, especially in terms of priority and quality of service
	- The moden ToS consists of 2 parts:
		1. **DSCP (Differentiated Services Code Point) (6 bits)**:
			- Tells routers how to prioritize packets
			- DSCP values:
				1. `0`(best effort) - Normal traffic
				2. **Expedited Forwarding (EF)** 
				3. **Assured Forwarding (AF)**
		2. **ECN (Explicit Congestion Notification) (2 bits)**
			- Allows routers to signal congestion **without dropping packets**
			- ECN bits:
				1. `00` - Not ECN-capable
				2. `10`/`01` - ECN-capable
				3. `11`-Congestion experienced
- Safeguards:
	- Header checksum protects header integrity
		- Guards against header corruption on lower layer
	- **Time to Live** limits how far a packet can travel
		- After 256 hops, the packet is dropped
		- Guards against routing issues (loops etc.)
- **Protocol (8-bit)** - name of the protocol to which the data is to be passed
- Fragmentation happens if a packet is too large for a given connection
	- Packet is split into 2 or more packets
	- Recipient re-assembles the fragments
- Fragments are routed as separate packets
	- Might take diffrent routes, arrive out-of-order, etc.
- _Identification_ is the same across all fragments
	- Refers to the original packet
- _Flags_: whether this is **not** the last packet (**More Fragments Flag**)
	- _Null bit_
	- _Don't Fragment_
	- _More Fragments_
- Fragment offset: This fragment's position within the original message

### IPv4 Fragmentation

![[CON - IPv4 Fragmentation.png]]

- Different network links support different maximum packet sizes called ***MTU(Maximum Transmission Unit)***. If an IP packet is bigger than the MTU of the next link, it cannot pass. To avoid dropping the entire messagen, IPv4 has **Fragmentation mechanism** which splits a large IP packet into smaller pieces
#### Basic Idea
- The sender takes the original big packet --> Slices into multiple **smaller packets (fragments)** --> send each fragment individually
- The receiver uses fragment metadata in the IP header to reassemble fragments back into the original packet
	> During the fragmentation phase, fragments may take different routes, arrives out of order, arive some late or get lost
	
#### Issues
- Because an Ipv4 packet has 16-bit ID field, so only 65536 different ID values can be used at the same time. The challenge is: **what if we want more than 65536 ID values to be sent in a moment?**
- Suggestion would be: "Why can't IDs just be reused immediately?"
	- Because IPv4 doesn't have acknowledgements or signaling for fragment reassembly (= The sender doen't know when the receiver is done reassembling) ---> The sender must wait before reusing an ID - long enough to guarantee that old fragments in the network have DISAPPEARED
- That waiting time is called ***TTL EXPIRATION (TIME-TO-LIVE)***
	- IPv4 packets have a TTL value - usally 64, 128, 255 seconds (= A packet can stay alive in the network for up to TTL seconds before being dropped)
- 64436 packets / 128 seconds = 512 packets per second
	- You can send 512 packets every 1 second
	- Those packets then remain "in flight" somewhere in the network for up to 128 seconds --> only after 128 seconds do those same ID values become safte to reuse

#### Another issues
- Fragmentation splits TCP/UDP transmission units across IP packets
	- Only the first fragment has the transport layer header (= the other fragments don't contain any info about port numbers, TCP state, etc.)
		- What if the fragment 1 could itself be chopped so small that even it fails to contain a TCP header
	- PAT rewrites IP + port numbers, but **second and later fragments contain no port numbers** --> router/ firewall cannot know: "Which internal device should I forward fragment 2 or 3 to?"
	- Firewalls can't effectively filter these packets either
		- Firewalls normally inspect **TCP flags, Port numbers, Application  Protocol Data**, but only fragment 1 contains this info, later fragments just look like raw data with no security information --> A malicious payload could be hidden in later fragments --> bypassing security inspection
- Reassembly is very fragile
	- The receiving system must store fragments until all pieces arrive/ timeout happens
	-  How long should fragments be kept around for?
		- Attacker sends first fragment, never sends the rest --> Receiver must store it, wasting memory --> Denial-of-Service Attacks (Fragment flooding)
	- How do you handle overlapping fragments?
		- Attackers can delibrately send overlappping pieces
#### Alternatives
##### Path MTU discovery
- Detect the largest packet size that can be sent unfragmented

###### How it works
- Sender sends **DF (Don't fragment) bit in IP header** (= "Routers, do NOT fragment my packets. Drop them instead if they're too big")
- Sender sends a full-size byte packet (e.g. 1500 Bytes)
- A router along the path sees: 
	- Packet is too large
	- DF flag is set --> must drop the packet
- Router sends back **ICMP (= Internet Control Message Protocol)** message
```
Type: Destination Unreachable
Code: Fragmentation Needed (but DF set)
Includes MTU value (ex: 1400)
```
- Sender learns: "Ah -- I must reduce packet size to <= 1400 bytes"
- Sender reduces packet size and retransmits

### Network Address Translation
- Allows multiple devices in a private network to access the internet using a single public IP address
	- Translate private IPs to public and vice versa
	- Prevents IPv4 address exhaustion
> **IPv4 provides only 2³² (about 4.3 billion) addresses, which is insufficient considering the massive number of devices connected to the Internet. NAT prevents IP exhaustion by enabling thousands of private devices to share a limited number of public IP addresses**

#### Working of NAT
1. A device sends a request → reaches the NAT-enabled router.
2. Router replaces the private IP with its public IP and assigns a unique port.
3. NAT stores this mapping in the NAT table.
4. When the server responds, NAT uses the stored entry to send the packet to the correct internal device.

### Trasmission Mode/ Communication mode
#### Definition:
- Transferring data between 2 devices
#### 1. Simplex Mode
- Sender can send the data but the sender unable receive the data
- Example: Keyboard
#### 2. Half-Duplex Mode
- Sender can send the data and also receive the data one sequentially
- It is a bidirectional communication but limited to only one at a time
#### 3. Full-Duplex Mode
- Sender can send the data and also can receive the data simultaneously
- It is dual way communication that is both way of communication happens at a same time
- Example: Telephone Network

