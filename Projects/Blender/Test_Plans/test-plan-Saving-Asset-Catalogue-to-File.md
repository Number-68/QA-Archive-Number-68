# Test Plan: Saving a list of asset catalogues to a file.
## Description
This test validates Blender’s `listing_asset_catalogs.write()` function, which generates a local cache file representing asset catalog metadata. The test constructs sample catalog entries—including edge‑case folder names—and verifies that the resulting cache file is correctly formatted, sanitized, and sorted.
## Objective
Ensure that writing remote‑library asset catalogs produces a correctly formatted, sanitized, and alphabetically sorted cache file that Blender can reliably read when syncing catalogs from external sources.
## Scope
### In Scope
- Sanitization of catalog paths (newline removal, colon handling, special‑character replacement).
    
- Alphabetical sorting of catalog entries by sanitized path.
    
- Correct header generation, including asset library name.
    
- Output format validation (`UUID:path:simple_name`).
    
- File creation in a temporary directory.
    
- Cleanup of generated files after execution.
### Out of Scope
- Empty catalog lists.
    
- Problematic or invalid characters (e.g., emojis, tabs).
    
- Duplicate paths or missing UUIDs.
    
- Large input sets or performance behavior.
    
- Overwrite behavior or file‑write error handling.
    
- Missing asset‑library metadata.
    
- Nonexistent output directory handling.
    
- Negative or malformed inputs.
## Environment
- **OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
- **Test Path:** `blender/tests/python/assets/remote_library_listing/listing_asset_catalogs_test.py`
## Approach
The test is executed automatically via Blender’s Python test suite (`ctest`). It generates temporary asset catalog data—including edge‑case folder names—invokes `listing_asset_catalogs.write()`, and inspects the resulting cache file. Assertions verify header correctness, sanitization rules, alphabetical ordering, and output formatting. The temporary directory and generated files are cleaned up after execution. The test artifact is the automated script located at the path above.
## Test Artifact
- **Automated Script:** `blender/tests/python/assets/remote_library_listing/listing_asset_catalogs_test.py`
