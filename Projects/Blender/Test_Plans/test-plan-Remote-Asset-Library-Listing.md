# Test Plan: Remote Asset Library Listing - Downloader & Sanitization Tests
## Description
This test plan covers the automated validation of Blender's ability to download a remote asset library index page and sanitize its contents – specifically correcting asset/file counts and converting unsafe file paths (absolute, UNC, directory traversal, mixed slashes) into safe, relative POSIX paths. The tests also verify platform‑agnostic path parsing and URL‑style path normalization.
## Objective
Ensure that the `listing_downloader.RemoteAssetListingDownloader._sanitize_asset_page()` method, along with its helper functions `_str_to_path_multiplatform()` and `_sanitize_path_from_url()`, correctly:

- Replace absolute, UNC, and malicious traversal paths with safe relative paths.
    
- Fix incorrect asset_count and file_count values.
    
- Write the sanitized JSON back to disk only when changes are necessary.
    
- Handle Windows, POSIX, and UNC path strings across platforms.
    
- Normalize percent‑encoded URL paths and resolve `..` components.
## Scope
### In Scope
- Correctness of `_sanitize_asset_page()` for:
    
    - Absolute paths (`C:\temp\file.blend`, `/temp/file.blend`)
        
    - UNC paths (`\\NAS\share\file.blend`, `//NAS/share/file.blend`)
        
    - Directory traversal attempts (`../../../etc/passwd`)
        
    - Mixed slash styles
        
    - Already‑correct paths (no rewrite)
        
- Correctness of `_str_to_path_multiplatform()` for Windows, POSIX, UNC, and relative path strings.
    
- Correctness of `_sanitize_path_from_url()` for:
    
    - Leading slash removal
        
    - `..` resolution
        
    - Percent‑decoding (`%2F`, `%2E`)
        
    - Empty string → `"."`
        
- File system interactions: temporary file creation, rewriting, and cleanup.
### Out of Scope
- Network download failures (timeouts, HTTP errors) – these are not simulated.
    
- Corrupt or invalid JSON parsing.
    
- Performance under very large catalogs (thousands of entries).
    
- Integration with the actual remote server or authentication.
    
- UI or user feedback mechanisms.
## Environment
- **OS:** Ubuntu 22.04 LTS (virtual machine)
    
- **CPU:** 4 cores — AMD Ryzen 5 2400G @ 3.60 GHz
    
- **RAM:** 8 GB allocated
    
- **Program Version:** Blender 5.2.0‑alpha (latest source build)
    
- **Execution Method:** Automated Python script
    
    - Path: `tests/python/assets/remote_library_listing/listing_downloader_test.py`
## Approach
Automated execution using the provided Python script. The script will be run against a temporary directory where it creates and reads JSON files simulating remote asset library data. Observations will focus on whether Blender correctly sanitizes bad file paths (absolute paths, UNC paths, directory traversal attempts), fixes incorrect asset counts, and only rewrites the JSON file when necessary. The test will verify both in‑memory changes and the final written file, ensuring no leftover files remain after execution.
## Test Artifact
- **Automated Script:** `tests/python/assets/remote_library_listing/listing_downloader_test.py`
