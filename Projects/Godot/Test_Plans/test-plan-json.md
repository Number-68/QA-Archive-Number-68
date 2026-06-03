# Test Plan: JSON – Stringify and Parse Round‑Trip, Edge Cases, and Serialization Precision

## Description
This test suite validates Godot's `JSON` class for converting Variant data to JSON strings and parsing JSON strings back to Variant. It covers single data types (null, bool, int, float, string), arrays (empty, flat, nested, self‑referential, max recursion), dictionaries (empty, single entry, nested, self‑referential, max recursion), escape sequences (valid and invalid), non‑finite numbers (inf, -inf, nan), and serialization precision (default vs. full precision for floating‑point and large integers). Error handling for malformed JSON is also checked.
## Objective
Ensure that `JSON::stringify` produces compliant JSON for all Variant types, handles infinite recursion by replacing recursive references with placeholders (`"[...]"`, `"{...}"`), respects indentation and full precision flags, correctly escapes control characters, and that `JSON::parse` successfully reads valid JSON (including single data types, arrays, dictionaries, and escape sequences) while rejecting invalid escape sequences. Also verify round‑trip consistency for non‑finite numbers (inf becomes `1e99999`, etc.) and that serialization of large integers matches expected strings.
## Scope
### In Scope
- Stringify: null, bool, int, float, string (escaped: `\\`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`, `\"`).
    
- Stringify arrays: empty, flat, nested, indented with `\t`, self‑referential (`[...]`), max recursion depth.
    
- Stringify dictionaries: empty, single entry, nested, indented, self‑referential (`{...}`), max recursion.
    
- Stringify non‑finite floats: `inf`, `-inf` → `1e99999`/`-1e99999`; `nan` → `null`.
    
- Stringify full precision: `full_precision=true` uses scientific notation for large/small numbers; default precision truncates.
    
- Parse: `null`, `true`, `false`, integer, float, string, arrays (complex), objects (dictionaries).
    
- Parse escape sequences: `\"`, `\\`, `\/`, `\b`, `\f`, `\n`, `\r`, `\t`, `\u0020` → space; invalid escapes (e.g., `\c`) return `ERR_PARSE_ERROR`.
    
- Round‑trip: non‑finite numbers parsed back to `INF`/`-INF`/`nil`.
### Out of Scope
- Non‑standard JSON extensions (single quotes, trailing commas, comments) – intentionally not tested.
    
- Performance or memory.
    
- Streaming or incremental parsing.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_json.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` calls `JSON::stringify` on Variant structures and compares the result with expected literal strings. For parsing, `JSON::parse` is called on JSON strings, and the resulting `get_data()` Variant is checked for equality with expected values. Error conditions suppress printed errors temporarily via `ERR_PRINT_OFF`/`ON`. Escape sequence tests iterate over ASCII ranges. Serialization precision tests compare output against pre‑computed strings for edge values (DBL_MAX, 1/3, etc.). All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_json.cpp`
