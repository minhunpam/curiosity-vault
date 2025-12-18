- Division of responsibilities
	- I don't care how my envelope gets there
	- The post office only needs to care about envelopes

- In networking, each layer has their own responsibilities and doesn't need to care what other layers do
- Modern internet uses the TCP/IP protocol suite

## The TCP/IP Model
- Has 4 layers:
1. Link Layer
	- Send a chunk of data to a directly connected computer
![[CON - link layer.png]]
2. Internet layer
	- Route a chunk of data to a remote computer along a series of direct links
3. Transport layer
	- Transmit a structured bit stream  across the internet
4. Applicaton layer
	- Offer services without having to worry about details

## Link layer
- Differences between mediums:
	- Shared medium
	- Switched medium
### Example: Wifi (IEEE 802.11)
- Shared medium: wireless audio
	- If 2 nodes send at the same time, the signals are garbled
- No collision detection
	- If a node is sending, it cannot listten for transmissions at the same time
- Collision avoidance (CSMA/CA)
	- Imperfect due to the hidden node problem
	- Example:
- Central access point
	- Nodes communicate via the AP
- Data is acknowledged
	- Collision -> no acknowledgement -> Data re-sent

### Addressing
- An address identifies a destination
	- Shared medium: recipients can recognize messages
	- Switched medium: we know where to forward messages to
- **MAC address:** 48-bit identifier

### Ethernet: Switching
- Ethernet switches understand Link Layer Data
- Record source addresses to build map address <-> port
- Only forward  packets to the appropriate port
	- Minimize wasted bandwidth
	- No collisions possible


## Checksums 
- Is a fixed-size value (4 Bytes)
- allows us to detect errors
	- Each message is sent with its correct checksum
	- If random bits get flipped, the checksum is no longer correct!
> Checksums don't help against an intelligent attacker!

### Ethernet: Switching Loops
- Multiple switches can be interconnected to form one big network
- STP (Spanning Tree Protocol)
	- Supported by professional switches
	- Automatically disables redundant links until needed


## Network Layer
### IPv4
- Internet Protocol, version 4
- Foundation of today's internet

#### Ipv4 Addressing
- 32-bit address
	- Notation: b
- 32-bit subnet mask
	- all ones, followed by all zeros
	- Splits address into network prefix and host number
- All hosts with the same network prefix form a subnet
- Hosts within the same subnet can communicat  directly
	- They're in the same link layer network
- Subnet masks do not need to be full bytes
	- 