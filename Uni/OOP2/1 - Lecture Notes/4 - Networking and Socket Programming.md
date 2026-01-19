## Networking?
- A collection of connected devices that can communicate with one another and shared resources and information
- **Nodes** are any connected devices which send/receive or forward data (e.g. computer, server, printer, . . . ).
- **Networking Devices** manage and support networking functions (e.g. routers, switches, access points, . . . ).
- **Transmission Media** The physical or wireless medium through which data travels.

## Networking Layers
**OSI model vs. TCP/IP model**
![[OOP2 - OSI model vs TCP IP model.png]]
- **Applciation layer**:
	- What the user directly interacts with (e.g., HTTP for web, FTP for file transfer, our Chat Protocol)
	- Handles data formatting, encryption (if implemented)
- **Transport layer**:
	- Provides end-to-end communication services
	- **TCP vs UDP**: We use TCP via Java sockets
- **Internet layer**:
	- Handles addressing and routing data across different networks (e.g, IP addresses)
	- Find the best path for data packets
- **Network inteface layer**:
	- Deals with the physical transmission of data on specific network medium (Ehternet, Wi-Fi, fiber)
	- Translates IP packets into actual electrical/ light signals

## Client-Server Model Architecture
- A central, "always-on" server provides data or services to multiple clients that initiate requests.
- Efficiently centralizes resources, data, and management, allowing many simple clients to share a single, powerful, and (hopefully) secure application.
- **Server**: Listens for and responds to requests (e.g., a web server, our chat server).
- **Client**: Initiates requests (e.g., your web browser, your chat client).

## Key Networking Terminology
- **IP Address**: Identifies the machine (e.g, `127.0.0.1` for localhost)
- **Port number**: Identifies the specific **application/ process** on that machine
	- Range: 0 - 65535
	- A single computer can run many network programs at once: Web server, Email server, SSH, Database, Games - **All share one IP address**. **Ports tell incoming data which program should receive it**
	- Common well-know ports:
		- 80 - HTTP
		- 443 - HTTPS
		- 22 - SSH
- **Socket**: is an endpoint of a network communication, defined by an IP address, a port number, and a protocol
	- Example: `(192.168.1.10, 80, TCP)`
	- In Java, this is the object we create and use to send and receive data (e.g: `new Socket("10.0.0.5", 6767)`)

## TCP vs. UDP
- **TCP (Transmission Control Protocol)**
	- **Connection-oriented**: A connection is established first, TCP-Handshake
	- **Reliable & Ordered**: Guarantees that messages arrive and in right order, can be slower
	- **Use case**: Web Browsing, file transfer, E-mail
- **UDP (User Datagram Protocol)**
	- **Connectionless**: "Fire and forget"
	- **Unreliable & Unordered**: Fast, but no guarantees for message arrival
	- **Use case**: Video streaming, gaming

## Networking in Java
- Java's `java.net` package provides a "high-level" API for networking
- It abstracts away the complex details of the OS and network stack
- **The big idea: we treat the network connection just like any other I/O Stream**
	- An I/O stream is a flow of data between a program and an exterenal source/destination
- We get an `InputStream` to read from the network
- We get an `OutputStream` to write to the network
> **This is very similar to reading/writing files, just over the Internet!**

### Server side:
- `ServerSocket(port)`: listens for connections on a specific port
- `Socket clientSocket = serverSocket.accept()`: waits for a client to connect. This is a blocking call, use a new (virtual) thread or reactive programming approach!

### Client side:
- `Socket socket = new Socket(ipAddress, port)`: connects to the server

### How do we communicate?
- `socket.getInputStream()` -> `BufferedReader`: To read text from socket
- `socket.getOutputStream()` -> `PrintWrite`: To write text to the socket

## Advaned Java Networking (interest only)
- **Encrypted Sockets (`SSLSocket`)**:
	- `SSLSocket` and `SSLServerSocket` automatically handle encryption (SSL/TLS) -> HTTPs
- **UDP sockets** (`DatagramSocket`):
	- For connectionless (UDP) communication
	- You send "datagram packets" -- no guarantees they arrive
- **Non-blocking I/O** (`java.nio`):
	- `socket.accept()` and `reader.readLine()` are blocking. They freeze the thread
	- This is fine for one client, but terrible for thousands
	- `java.nio.channels` (new I/O) allows a single thread to "multiplex" and manage thousands of connections
