# Test Plan: StreamPeerGZIP – GZIP/DEFLATE Compression and Decompression


## Description
This test suite validates Godot’s `StreamPeerGZIP` class, which provides compression and decompression using the GZIP or DEFLATE algorithms. It tests that a simple string and a larger (2500‑byte) random block can be compressed and then decompressed back to the original data, that state management (starting twice, finishing without data, clearing) returns appropriate errors, and that invalid parameters (zero/negative buffer sizes) are rejected.
## Objective
Ensure that `StreamPeerGZIP` correctly compresses and decompresses data with both GZIP and DEFLATE formats, that the round‑trip produces identical output, that all error conditions (already started, invalid buffer size, not started, finish after clear, get before any data) return the expected error codes, and that the class can handle arbitrary binary data (including random bytes) without corruption.
## Scope
### In Scope
- Compression and decompression of a small ASCII string (`"Hello World!!!"`).
    
- Compression and decompression of a 2500‑byte random binary block.
    
- Both GZIP (`is_deflate = false`) and DEFLATE (`is_deflate = true`) modes.
    
- Proper state transitions: start → put_data → finish → get_data → clear → start again.
    
- Error detection:
    
    - Starting compression or decompression twice (`ERR_ALREADY_IN_USE`).
        
    - Starting with buffer size ≤ 0 (`ERR_INVALID_PARAMETER`).
        
    - `put_data` or `get_data` with negative size (`ERR_INVALID_PARAMETER`).
        
    - Using `put_data` before starting (`ERR_UNCONFIGURED`).
        
    - `finish` after `clear` or in decompression mode (`ERR_UNAVAILABLE`).
        
    - `get_data` when no data has been compressed/decompressed (`ERR_UNAVAILABLE`).
### Out of Scope
- Streaming over network or files.
    
- Performance or memory usage.
    
- Other compression formats (e.g., Zstd, Brotli).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_stream_peer_gzip.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. It creates a `StreamPeerGZIP` instance, calls `start_compression`/`start_decompression`, writes data with `put_data`, finishes compression, reads compressed data with `get_data`, clears, then decompresses and verifies the result. Edge cases are tested with `ERR_PRINT_OFF`/`ON` to suppress expected error messages. Random data is generated with `Math::random`. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_stream_peer_gzip.cpp`
