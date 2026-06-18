# Test Plan: UDPServer – UDP Packet Reception, Sending, and Multiple Client Handling



## Description
This test suite validates Godot's `UDPServer` class, which implements a UDP server for receiving packets from multiple clients. It tests server instantiation and listening, accepting a "connection" (UDP peer), exchanging Variant data (strings and floats) between client and server, handling multiple clients concurrently, and stopping the server to reject new packets. The test uses real localhost networking with polling to check for available packets.
## Objective
Ensure that `UDPServer` correctly starts and stops listening, that it can receive UDP packets from clients and send responses, that `poll()` detects incoming packets from multiple clients, that `take_connection()` returns a valid `PacketPeerUDP` for each client, that data round‑trips correctly (Variant serialization), and that stopping the server prevents further packet reception.
## Scope
### In Scope
- Server instantiation and listening on localhost:12345.
    
- Client creation and connecting to the server.
    
- Sending a packet (Variant string) from client to server.
    
- Polling and accepting a connection (returning a `PacketPeerUDP`).
    
- Receiving the Variant on the server side and verifying it.
    
- Sending a Variant (float) from server back to client.
    
- Client polling to check available packets and receiving the response.
    
- Multiple clients (5) sending unique strings, server responding with computed floats.
    
- Packet ordering and content verification.
    
- Server stop: prevents new packet acceptance.
### Out of Scope
- External network (uses localhost only).
    
- IPv6 or other IP families.
    
- Performance or concurrency beyond 5 clients.
    
- Lossy networks or packet reordering (assumes reliable local delivery).
    
- TCP or other protocols.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_udp_server.cpp`
## Approach
The test uses Godot's built‑in doctest framework. Helper functions `create_server`, `create_client`, and `accept_connection` handle setup with assertions. `wait_for_condition` polls with small delays until a condition is met (or timeout). Data transfer tests use `put_var`/`get_var` with `CHECK_EQ`. Multiple client tests iterate over vectors of clients, sending unique strings, receiving responses, and verifying both send and receive sides. The test runs automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_udp_server.cpp`
