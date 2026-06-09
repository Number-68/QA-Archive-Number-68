# Test Plan: PCKPacker – PCK Archive Creation and File Packing
## Description
This test suite validates Godot’s `PCKPacker` class, which creates PCK (package) files used for game data archives. It tests starting a PCK file with default and invalid parameters (zero alignment, empty key), packing an empty PCK, packing files from disk (including nested directories, overwriting entries, and files with spaces in paths), packing from in‑memory buffers, and verifying that the resulting PCK file has a valid size range.
## Objective
Ensure that `PCKPacker` can start a PCK archive with correct alignment (default 32, non‑zero, valid key), add files by source path (absolute or relative) and destination path (creating intermediate directories), override existing entries before flushing, add files from raw buffers, and flush the archive without errors. Verify that the produced PCK file is readable, contains a proper header, and has a reasonable file size.
## Scope
### In Scope
- Starting an empty PCK with default parameters → success, file size 100‑500 bytes.
    
- Starting with zero alignment → error.
    
- Starting with empty encryption key → error.
    
- Adding files from the filesystem (relative to executable base dir) with nested destination paths (including spaces).
    
- Overwriting a previously added file before flush.
    
- Adding a file from an in‑memory buffer (UTF‑8 content).
    
- Flushing the PCK → success.
    
- Final PCK file size between 18000‑27000 bytes for a test set of known files.
### Out of Scope
- Encryption or decryption (only key validation is tested, not actual encryption).
    
- Extracting or reading PCK files (only packing).
    
- Performance or memory usage.
    
- Large numbers of files or huge buffers.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_pck_packer.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. A `PCKPacker` instance is created. The test calls `pck_start` with various arguments, checking return codes. For a valid PCK, it adds several test files (using existing engine source files like `version.py`, `icon.png`, `icon.svg`) and a buffer‑based file, then flushes. The resulting PCK is opened with `FileAccess` and its size is checked against expected ranges. Error cases suppress output with `ERR_PRINT_OFF`/`ON`. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_pck_packer.cpp`
