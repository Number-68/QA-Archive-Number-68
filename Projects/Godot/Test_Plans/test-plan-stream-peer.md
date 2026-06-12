# Test Plan: StreamPeerBuffer – Data Serialization and Deserialization
## Description
This test suite validates Godot’s `StreamPeerBuffer` class, which implements a memory‑based stream peer for reading/writing primitive data types, strings, and Variants. It checks that integer types (8/16/32/64‑bit, signed/unsigned), half‑precision floats, single/double floats, strings (plain and UTF‑8), and Variants can be written to a buffer and read back correctly with both little‑endian (default) and big‑endian mode. Error cases (reading string from empty buffer) return empty strings.
## Objective
Ensure that `StreamPeerBuffer` correctly serializes and deserializes all supported data types, respects endianness settings, properly handles half‑precision float conversion (lossy), and returns empty strings when reading string data from an empty buffer.
## Scope
### In Scope
- Default endianness: little‑endian (false).
    
- `set_big_endian(true)` switching to big‑endian.
    
- Writing/reading: `int8_t`, `uint8_t`, `int16_t`, `uint16_t`, `int32_t`, `uint32_t`, `int64_t`, `uint64_t`, `half` (16‑bit float), `float`, `double`, `String`, `utf8 String`, `Variant`.
    
- Round‑trip value equality (half‑float with approximation).
    
- Error case: `get_string()` / `get_utf8_string()` on empty buffer returns empty string.
### Out of Scope
- Other `StreamPeer` implementations (TCP, UDP, etc.).
    
- Performance or memory usage.
    
- Partial reads/writes or boundary conditions.
    
- Concurrent access.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_stream_peer.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. A `StreamPeerBuffer` is instantiated, cleared, and each data type is written using `put_*` methods, then `seek(0)` and read back with corresponding `get_*` methods. Results are compared with `CHECK_EQ` or `CHECK(doctest::Approx(...))`. Endianness is tested both default and after `set_big_endian(true)`. Error cases suppress output with `ERR_PRINT_OFF`. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_stream_peer.cpp`
