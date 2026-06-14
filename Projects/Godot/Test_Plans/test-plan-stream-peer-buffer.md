# Test Plan: StreamPeerBuffer – Memory Buffer Operations and Data Management
## Description
This test suite validates Godot’s `StreamPeerBuffer` class, an in‑memory stream peer. It checks that a newly created buffer has zero size, position, and available bytes; that `seek()` moves the cursor correctly with bounds checking (negative or beyond‑size seeks are ignored); that `resize()` changes the buffer size while preserving position; that `get_data_array()` and `set_data_array()` correctly retrieve and replace the entire buffer content; that `duplicate()` creates an independent copy; and that edge cases (putting zero bytes does nothing; getting more bytes than available returns an error and leaves position unchanged).
## Objective
Ensure that `StreamPeerBuffer` correctly manages its internal byte array: seeking respects bounds and ignores invalid offsets; resizing adjusts capacity and does not reset the cursor; data array access returns the exact stored bytes; setting a new array replaces the buffer content; duplication produces a separate buffer with the same data; and that zero‑size writes are harmless while over‑reads return `ERR_INVALID_PARAMETER`.
## Scope
### In Scope
- Initial state: size 0, position 0, available bytes 0.
    
- `seek()`: valid positions, negative (ignored), beyond end (ignored).
    
- `resize()`: changes size; does not change position if within new size; available bytes adjust.
    
- `get_data_array()` returns correct byte vector.
    
- `set_data_array()` replaces buffer content and resets position to 0.
    
- `duplicate()` creates new buffer with same data.
    
- `put_data()` with length 0 does nothing.
    
- `get_data()` requesting more bytes than available returns `ERR_INVALID_PARAMETER`.
### Out of Scope
- Other data type serialization (handled by `StreamPeerBuffer` as base class; this file focuses on the buffer itself).
    
- Endianness or half‑float conversion.
    
- Performance or concurrency.
    
- Error handling for out‑of‑memory (unlikely for small test data).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_stream_peer_buffer.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` instantiates a `StreamPeerBuffer`, performs operations (putting bytes, seeking, resizing, duplicating, etc.), and asserts the resulting size, position, available bytes, or data content using `CHECK_EQ`. Error conditions suppress output with `ERR_PRINT_OFF`/`ON` while verifying that ignored operations do not change state. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_stream_peer.cpp`
