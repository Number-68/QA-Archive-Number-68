# Test Plan: FileAccess – CSV Parsing, Line Endings, Float I/O, Compression, and Cursor Positioning
## Description
This test suite validates Godot's `FileAccess` class for file I/O operations. It covers reading CSV files with various delimiters and quoted fields (including escaped quotes and multiline values), reading files as UTF‑8 strings across different line endings (Unix `\n`, Windows `\r\n`, legacy macOS `\r`, and mixed), storing/retrieving single‑precision and half‑precision floating‑point values in both little‑ and big‑endian formats, reading a compressed FastLZ file, and testing cursor movement (`seek`, `seek_end`, `get_position`) including edge cases (beyond file size, negative offset).
## Objective
Ensure that `FileAccess` correctly handles CSV parsing with standard and custom delimiters, properly interprets different line ending styles when reading text as UTF‑8, correctly reads and writes 32‑bit and 16‑bit floats with endianness control, successfully decompresses FastLZ data, and manages file cursors predictably (initial position, absolute seeks, end‑relative seeks, bounds checking).
## Scope
### In Scope
- CSV reading: default comma delimiter, custom delimiter (`~`, `\t`), quoted fields, escaped quotes, empty strings, multiline values, and handling of missing translations.
    
- UTF‑8 string reading from files with `\n`, `\r\n`, `\r`, and mixed newlines; verifying `get_line()` produces correct lines without stray characters.
    
- Reading/writing 32‑bit float in little and big endian (via `set_big_endian`), with checksum comparison.
    
- Reading/writing half‑precision float (16‑bit) with endianness control.
    
- Reading compressed FastLZ file (exactly 4096 bytes of zeros) and verifying full and partial reads.
    
- Cursor operations: `get_position()` initial zero, `seek()` absolute, `seek_end()` with positive/negative offsets, and handling of positions beyond file size or negative (ignored).
### Out of Scope
- Other compression methods (Zlib, GZip).
    
- Writing compressed files.
    
- File permissions, locking, or directory operations.
    
- Performance or large files.
    
- Platform‑specific path handling beyond test data paths.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_file_access.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` opens test data files from `TestUtils::get_data_path()`. For CSV and line‑ending tests, it reads rows or lines and compares strings. For float tests, it reads pre‑generated binary files, compares values, writes new files, and compares SHA‑256 checksums. The compressed file test reads a FastLZ file and verifies decompressed zero bytes. Cursor tests seek and assert positions, checking behavior at boundaries. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_file_access.cpp`
