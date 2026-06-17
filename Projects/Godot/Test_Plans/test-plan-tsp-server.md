# Test Plan: TCPServer – TCP Server Lifecycle, Client Connections, and Data Transfer

## Description
This test suite validates Godot's `TCPServer` class, which implements a TCP server that listens for incoming connections. It tests server instantiation and listening, accepting a single client connection with data exchange (sending/receiving strings and floats), handling multiple simultaneous clients, stopping the server to reject new connections, and proper disconnection of clients. The test uses real localhost networking with time‑based synchronization to handle asynchronous connection states.
## Objective
Ensure that `TCPServer` correctly starts and stops listening, accepts incoming client connections, and that each accepted client can exchange data (strings, floats) with the server. Verify that multiple clients can connect and interact concurrently, that stopping the server prevents further connections, and that disconnecting a client (either side) updates status correctly and prevents further I/O.
## Scope
### In Scope
- Server instantiation and listening on a local port (12345).
    
- Client connection establishment and acceptance.
    
- Data transfer: client → server (string), server → client (float).
    
- Multiple clients (5) connecting, accepting, and exchanging data.
    
- Server stop: prevents new connections, existing clients disconnected.
    
- Client disconnection: manual `disconnect_from_host` sets status to `STATUS_NONE`.
    
- Error handling: reading after disconnection returns empty string.
    
- Use of `wait_for_condition` helper to poll for async state changes (up to 2 seconds).
### Out of Scope
- External network (uses localhost only).
    
- IPv6 or other IP families.
    
- Performance or concurrency beyond 5 clients.
    
- SSL/TLS encryption.
    
- Timeout configuration (only briefly touched).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_tcp_server.cpp`
## Approach
The test uses Godot's built‑in doctest framework. A helper `create_server` starts a server on localhost:12345, `create_client` connects a client, and `accept_connection` polls until a connection is available. Another helper `wait_for_condition` loops with small delays until a lambda condition is met (or timeout). Data transfer tests use `put_string`/`get_string` and `put_float`/`get_float` with `CHECK_EQ`. Multiple client tests iterate over vectors of clients and server‑side peers. Timeout behavior is tested by temporarily reducing the connect timeout setting. All tests run automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_tcp_server.cpp`
