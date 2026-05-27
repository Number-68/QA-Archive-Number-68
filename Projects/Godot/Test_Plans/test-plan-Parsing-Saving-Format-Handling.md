# Test Plan: ConfigFile – Parsing, Saving, and Format Handling

## Description
This test suite validates Godot's `ConfigFile` class, which reads and writes INI‑style configuration files. It checks that well‑formatted files (with sections, key‑value pairs, multiline strings, Color/Vector2 literals, inline comments, and case‑sensitivity) parse correctly and return expected values. It also verifies that malformatted files (extraneous quotes, missing parameters, extra commas, duplicate keys) return a parse error. Finally, it tests saving a `ConfigFile` to disk and compares the generated content against an expected string, including Unicode keys and keys containing special characters (`=`).
## Objective
Ensure that `ConfigFile` correctly parses human‑friendly INI syntax, gracefully rejects malformed input, and saves in‑memory configuration data to a file with proper escaping of section names, keys, and values – preserving multiline strings, Godot‑style Color/Vector2 literals, and quoted keys containing non‑ASCII characters or special symbols.
## Scope
### In Scope
- Parsing well‑formed files: sections, string values (including multiline), `Color(...)` and `Vector2(...)` literals, inline comments (semicolon), case‑sensitive keys, and blank lines.
    
- Retrieving parsed values via `get_value()` and converting to native types (String, Color, Vector2, bool).
    
- Parsing an empty string (should succeed).
    
- Parsing malformed files: extra closing quote, missing Color parameter, extra comma in Vector2, duplicate key – should return `ERR_PARSE_ERROR`.
    
- Saving a `ConfigFile` with various data types: string (plain and multiline), Color, Vector2, bool, integer key (Unicode key string), key containing `=` character.
    
- Comparing saved file content to expected string (including proper quoting of keys with special characters).
### Out of Scope
- Encryption or password‑protected config files.
    
- Large file performance or memory usage.
    
- Cross‑platform path handling (test uses platform‑specific temporary paths, but only minimal).
    
- Interaction with `ProjectSettings`.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/input/test_config_file.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` creates a `ConfigFile` instance. The first test parses a hand‑crafted multi‑line string, then asserts each retrieved value using `CHECK_MESSAGE`. The second test attempts to parse a malformed string and checks that the error code is `ERR_PARSE_ERROR` (printing of errors is temporarily disabled). The third test sets values programmatically, saves to a temporary file (`config.ini` in `TEMP` on Windows, `/tmp/` otherwise), reads the file back, and compares the content against an expected string literal. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/input/test_config_file.cpp`
