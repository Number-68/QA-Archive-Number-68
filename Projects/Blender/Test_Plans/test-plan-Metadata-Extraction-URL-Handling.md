# Test Plan: Asset Metadate conversion and URL hash construction tests.
## Description
This test file validates two core components of Blender's upcoming Remote Asset Library feature. First, it checks that an asset's custom properties (strings, numbers, booleans, and arrays) are correctly extracted and converted into a structured AssetMetadataV1 model suitable for JSON serialization. Second, it verifies that asset download URLs are built correctly, including proper percent‑encoding of hash values, stripping of hash type prefixes (e.g., sha1:), and correct appending to URLs that may already contain query strings. The tests run inside Blender using a fresh factory startup scene (the default cube) and do not require external network access.
## Objective
The objective is to ensure that `asset_finder._get_asset_meta()` faithfully transforms an asset's custom property dictionary into the expected `AssetMetadataV1` structure, handling plain properties, arrays, and the absence of required fields (like `dimensions`), and that the resulting metadata can be serialized to JSON and back without data loss. Additionally, the tests confirm that `hashing.url()` builds correct download URLs by appending hash parameters appropriately, encoding special characters, stripping type prefixes, and preserving existing query strings, while accepting both `URLWithHash` objects and tuples as input.
## Scope
### In Scope
- Testing `_get_asset_meta` with plain properties (string, integer, float, boolean) and array properties.
    
- Deleting the required `dimensions` key to verify graceful `None` return.
    
- Serialization/deserialization round‑trip via `ValidatingParser`.
    
- Testing `hashing.url` with empty hash, special characters, prefixed hash, existing query strings, and tuple input.
### Out of Scope
- Actual network downloads or remote server interaction.
    
- Performance or memory usage under large custom property sets.
    
- Validation of hash algorithm correctness (e.g., does SHA‑1 match file content).
    
- UI or user preferences for remote asset libraries.
    
- Other functions in `cli_listing_generator_asset_finder` beyond `_get_asset_meta`.
## Environment
- - **OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
- **Test Path:** `blender/tests/python/assets/remote_library_listing/listing_generator_test.py`
## Approach
Automated execution using the provided Python script. The script starts Blender in background mode with a factory‑fresh startup file, creates an asset‑marked default cube, adds various custom properties (plain and array types), and verifies the extracted metadata matches expected `api_models` structures. It also tests the URL‑building function with multiple edge cases. All assertions are handled by the `unittest` framework; no manual intervention is required.
## Test Artifact
- **Automated Script:** `blender/tests/python/assets/remote_library_listing/listing_generator_test.py`
