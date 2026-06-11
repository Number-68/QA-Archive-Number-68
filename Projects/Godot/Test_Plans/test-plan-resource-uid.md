# Test Plan: ResourceUID – UID Encoding and Decoding to/from Text
## Description
This test suite validates Godot’s `ResourceUID` singleton, which converts between 64‑bit integer UIDs and human‑readable strings (`uid://...`). It checks that the maximum (`0x7fffffffffffffff`) and minimum (`0`) UIDs encode to the expected strings and decode back correctly, that invalid UIDs (`-1`, `-2`) encode to `"uid://<invalid>"` and decode to `-1`, and that a sample normal UID round‑trips successfully.
## Objective
Ensure that `id_to_text` and `text_to_id` are inverses for all valid UIDs (including boundary values), that invalid IDs are consistently handled (mapped to a sentinel string and decoding returns `-1`), and that strings without the `uid://` scheme decode to `-1`.
## Scope
### In Scope
- Encoding maximum UID (`0x7fffffffffffffff`) → `"uid://d4n4ub6itg400"`.
    
- Encoding minimum UID (`0`) → `"uid://a"`.
    
- Decoding those strings back to the original integers.
    
- Encoding invalid UIDs (`-1`, `-2`) → `"uid://<invalid>"`.
    
- Decoding `"uid://<invalid>"` → `-1`.
    
- Decoding a string without `uid://` scheme → `-1`.
    
- Encoding a normal UID (`8060368642360689600`) → `"uid://dm3rdgs30kfci"` and decoding back.
### Out of Scope
- UID assignment or file system operations.
    
- Thread safety or performance.
    
- Other string formats (only `uid://` scheme is tested).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_resource_uid.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. It calls `ResourceUID::get_singleton()->id_to_text` and `text_to_id` with the test values and compares results using `CHECK_MESSAGE`. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_resource_uid.cpp`
