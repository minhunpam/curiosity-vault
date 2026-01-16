- Goal:
	- Build a simple network stack in C/C++ that can be used to provide simple network functionality 
		- send ARP
		- ICMP replies
	- For every network layer, have to inspect the header fields
		- Figure out which protocol is encapsulated in the payload
		- Pass the payload to the next layer or properly respond to the request

## Network byte order vs host byte order
- **Host byte order** - how your CPU stores numbers (usually little-endian)
- **Network byte order** - how numbers are sent on the wire (always big-endian)
### The conversion functions
1. From network -> host (used when receiving packets):
	- `ntohs()` - network -> host short (16-bit/ 2 bytes)
	- `ntohl()` - network -> host long (32-bit/ 4 bytes)
2. From host -> network (used when creating packets):
	1. `htons()` - host -> network short (16-bit/ 2 bytes)
	2. `htonl()`- host -> network long (32-bit/ 4 bytes)

## 1. Ethernet
### `handle_packet()`
1. Log the source and destination MAC addresses
2. Check the frame header for the ethernet type to know if the frame contains an IPv4 packet or an ARP packet to forward the payload to the corresponding protocol handler. Otherwise, drop the frame

### `send()`
1. Create an Ethenet frame with all necessary header fields + the payload
	- The frame's size is sum of the size of the header and payload
	- Set each field using `memcpy` with correct offsets
2. Send it using the private method

## 2. ARP
### `handle_packet()`
1. Check if the ARP packet uses IP as network protocol and Ethernet as hardware protocol by comparing header fields `ar_hrd`, `ar_pro` with correct constants
2. Log request
3. Check if the request is targeted towards your protocol stack's IP address. If yes then respond by calling `send()`

### `send()`
- Create buffer on the stack for the packet
	- Set all needed header fields
	- Set all addresses to the packet body
- Log ARP reply
- Send to IPv4 handler

## 3. IPv4
> Asume no Packet fragmentation 

### `handle_packet()`
1. Log addresses
2. Check if the request is targeted towards your IP address
3. Check if the payload is ICMP by checking `ip_p` with the correct constant. Otherwise, stop processing
4. Foward the payload to the IMCP protocol

### `send()`
1. Create a buffer to store the packet
2. Set all necessary header fields
3. Log
4. Send

## ICMP
- `ping` - sends ICMP echo requests and waits for echo replies
### `handle_packet()`
1. Check if the `type` in the header is for ICMP echo request. If 1, log
2. Respond by calling `send()`

### `send()`
1. Create a buffer to store the packet
2. Set all necessary header fields and add set it + payload to the buffer
3. Log
4. Foward to the IPv4 layer


