# Test Plan: Marshalls – Low‑Level Binary Encoding and Decoding of Integers, Floats, Strings, and Variants

## Description
This test suite validates Godot’s low‑level marshalling functions (`encode_uint16/32/64`, `decode_uint16/32/64`, `encode_half/float/double`, `decode_half/float/double`, `encode_cstring`, and `encode_variant`/`decode_variant`). It covers endian‑aware encoding/decoding of unsigned integers (16/32/64‑bit), half/single/double precision floating‑point numbers, C strings, and Variants (including typed arrays and dictionaries). Tests verify correct byte ordering, size, and round‑trip consistency.
## Objective
Ensure that the marshalling utilities produce the expected byte sequences (little‑endian order, half‑float format, IEEE 754) and that decoding recovers the original values exactly. For Variants, confirm that type tags, flags (e.g., `HEADER_DATA_FLAG_64` for 64‑bit data), and container type information (for typed arrays/dictionaries) are correctly written and read, and that invalid data is rejected.
## Scope
### In Scope
- Encoding/decoding `uint16_t`, `uint32_t`, `uint64_t` (little‑endian).
    
- Encoding/decoding half (16‑bit) float (`0.33325195f`), single (`0.15625f`), double (`0.33333333333333333`).
    
- Encoding C string (includes trailing null).
    
- Variant encoding/decoding for: `nil`, 32‑bit int, 64‑bit int, single‑precision float, double‑precision float.
    
- Variant encoding/decoding of typed arrays (builtin `int`) and typed dictionaries (key `int`, value `int`).
    
- Error handling: insufficient buffer size, invalid type tag.
### Out of Scope
- Other Variant types (strings, objects, packed arrays, etc.).
    
- Performance or memory.
    
- Network or file I/O integration.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_marshalls.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` calls the encoding function with a known value, checks the returned size and byte array content (using `CHECK_MESSAGE` on individual bytes). Decoding tests read pre‑defined byte arrays and compare the decoded result with the original value. Variant tests encode a Variant into a buffer, verify header flags and data bytes, then decode and compare the Variant. Error paths suppress output with `ERR_PRINT_OFF` and check for `ERR_INVALID_DATA`. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_marshalls.cpp`
