# Test Plan: PacketPeerStream – Encoding Buffer Limits, Variant I/O, and Packet Buffer Transfer

## Description
This test suite validates Godot’s `PacketPeerStream` class, which wraps a `StreamPeer` to provide packet‑oriented transfer of Variants and raw data. It checks that the maximum encode buffer size is clamped between 1024 bytes and 256 MiB, rounded up to the next power of two; that `put_var`/`get_var` correctly send and receive Variants (including error handling when no stream peer is set); that oversize Variants cause `ERR_OUT_OF_MEMORY`; and that raw packet buffers can be written and read back correctly.
## Objective
Ensure that `PacketPeerStream` enforces sensible limits on its internal encode buffer (minimum 1024, maximum 256 MiB, power‑of‑two rounding), that `put_var` and `get_var` faithfully round‑trip Variant data through a `StreamPeerBuffer`, that appropriate errors are returned when the stream peer is missing or when the encoded data exceeds the buffer size, and that `put_packet_buffer`/`get_packet_buffer` correctly transfer raw byte buffers (including length prefix).
## Scope
### In Scope
- Default encode buffer size (8 MiB).
    
- Setting buffer size: below 1024 → clamped to 1024; above 256 MiB → clamped to 256 MiB; arbitrary value → rounded up to next power of two.
    
- Error returns when `get_var` called without a stream peer (`ERR_UNCONFIGURED`).
    
- Round‑trip `put_var`/`get_var` with a `StreamPeerBuffer`.
    
- `put_var` of a string larger than encode buffer → `ERR_OUT_OF_MEMORY`.
    
- `put_packet_buffer`/`get_packet_buffer`: writes length‑prefixed data, reads back correctly.
    
- Empty packet buffer write → no data written.
    
- `get_packet_buffer` when no packet available → `ERR_UNAVAILABLE`.
### Out of Scope
- Other `PacketPeer` implementations (e.g., UDP, TCP).
    
- Multithreading or concurrent access.
    
- Performance or network latency.
    
- Big‑endian vs little‑endian (tests assume little‑endian, but the implementation uses host order).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_packet_peer.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. A `StreamPeerBuffer` is used as the underlying stream. Each `TEST_CASE` creates a `PacketPeerStream`, sets the stream peer (or not for error cases), performs operations, and asserts results with `CHECK_EQ` and `CHECK`. Error prints are suppressed via `ERR_PRINT_OFF` when expecting failures. Buffer size clamping and power‑of‑two checks are tested directly on the `PacketPeerStream` instance. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_packet_peer.cpp`
