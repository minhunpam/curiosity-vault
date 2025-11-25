- A framework is based on a client-server model of remote procedure calls
- A client can directly call methods on a server application as if it were a local object

## Maven Depedencies
- group ID: `io.grpc`
- artifact IDs:
	- `grpc-netty`
	- `grpc-protobuf`
	- `grpc-stub`

## Kinds of Service methods
1. A simple RPC
	- client sends request to the server using the stub and waits for for a response to come back, just like normal function call

## Server
### Creating server
- 2 parts:
	- Overriding the service base class generated from our service definition
	- Running a gRPC server to listen for requests from clients and return the service responses
- Implement server `RouteGuideService` that extends the generated `RouteGuideGrpc.RouteGuideImplBase`

- response observer (server side) controls how the server sends data back to the client
	- `onNext(response)` - sends a message to the client
	- `onCompleted()` - tell the client "I'm done" - closes the stream
	- `onError(Throwable)` - sen an error
	```java
	responseObserver.onError(new RuntimeException("Something went wrong"));
	```

### Starting the server
- gRPC Server Port will start on port 9090