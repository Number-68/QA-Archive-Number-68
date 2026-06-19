# Test Plan: UDSServer – Unix Domain Socket Server Lifecycle and Data Exchange

## Description
This test suite validates Godot's `UDSServer` class, which implements a Unix Domain Socket (UDS) server for local inter‑process communication. It tests server instantiation and listening on a socket file, accepting client connections, exchanging data (strings and floats) between client and server, handling multiple concurrent clients, stopping the server to reject new connections, and proper disconnection of clients. The test uses real UDS connections over local socket files with polling and time‑based synchronization.
## Objective
Ensure that `UDSServer` correctly starts and stops listening on a UDS path, accepts incoming client connections, exchanges data (strings, floats) correctly, handles multiple clients simultaneously, stops accepting new connections when stopped, and properly disconnects clients (setting status to `STATUS_NONE`). Verify that different socket paths work correctly and that socket files are cleaned up after tests.
## Scope
### In Scope
- Server instantiation and listening on a socket path (`/tmp/godot_test_uds_socket`).
    
- Client connection establishment and acceptance.
    
- Data transfer: client → server (string), server → client (float).
    
- Multiple clients (5) connecting, accepting, and exchanging data.
    
- Server stop: prevents new connections, existing clients disconnected.
    
- Client disconnection: manual `disconnect_from_host` sets status to `STATUS_NONE`.
    
- Error handling: reading after disconnection returns empty string.
    
- Alternative socket path testing (`/tmp/godot_test_uds_socket_alt`).
    
- Socket file cleanup before and after tests.
### Out of Scope
- Network sockets (TCP/UDP).
    
- Windows or macOS (test is conditionally compiled for Unix only).
    
- Performance or concurrency beyond 5 clients.
    
- SSL/TLS encryption.
    
- File system permissions beyond existing socket file handling.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_uds_server.cpp`
## Approach
The test uses Godot's built‑in doctest framework, conditionally compiled only on Unix systems. Helper functions `create_server`, `create_client`, and `accept_connection` handle setup with assertions. `wait_for_condition` polls with small delays until a condition is met (or timeout). Data transfer tests use `put_string`/`get_string` and `put_float`/`get_float` with `CHECK_EQ`. Multiple client tests iterate over vectors of clients and server‑side peers. Socket files are created in `/tmp/` and cleaned up with `cleanup_socket_file`. The test runs automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_uds_server.cpp`
