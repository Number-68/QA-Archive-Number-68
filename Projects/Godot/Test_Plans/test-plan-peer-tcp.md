# Test Plan: StreamPeerTCP – TCP Socket Operations with Mock Networking
## Description
This test suite validates Godot's `StreamPeerTCP` class using a mock `NetSocket` implementation to simulate network behavior without requiring actual network connections. It tests basic TCP operations: accepting a socket, binding to a port, connecting, disconnecting, polling for status changes, and sending/receiving data. The mock socket allows precise control over available bytes and data buffers to verify state transitions.
## Objective
Ensure that `StreamPeerTCP` correctly manages a TCP socket's lifecycle: that it can accept a socket from a mock peer, bind to a valid port (rejecting invalid ports), disconnect gracefully, poll to detect connection establishment (moving from unconnected to connected), detect FIN (remote close) and transition to disconnected, and successfully send and receive data (byte‑by‑byte) through the underlying socket.
## Scope
### In Scope
- Accepting a mock socket with a peer IP/port.
    
- Binding to a port with valid and invalid port numbers (negative, > 65535).
    
- Disconnecting from host (closes the socket).
    
- Polling: connecting when unconnected, detecting FIN when available bytes = 0.
    
- Sending data via `put_data` (ASCII string) – data written to mock socket's send buffer.
    
- Receiving data via `get_data` – data read from mock socket's recv buffer.
    
- Mock socket controls: `_set_available_bytes`, `_set_send_data`, `_set_recv_data`.
### Out of Scope
- Actual network connections over real hardware.
    
- UDP, SSL, or other protocols.
    
- Performance or concurrency.
    
- Timeout handling (mock poll always returns OK).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_stream_peer_tcp.cpp`
## Approach
The test uses Godot's built‑in doctest framework with a custom `MockNetSocket` class that overrides all network methods to simulate behavior (send/receive one byte per call, control available bytes). Each `TEST_CASE` creates a `StreamPeerTCP` and `MockNetSocket`, accepts the socket, then performs operations (bind, disconnect, poll, put_data, get_data) and checks results with `CHECK_EQ` and `REQUIRE`. Error prints are suppressed when testing invalid inputs. All tests run automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_stream_peer_tcp.cpp`
