# Test Plan: View Layer – Scene Collection and Layer Collection Syncing with JSON Comparison

## Description
This test validates that scene collections and layer collections remain in sync when creating new subcollections, linking objects, and unlinking objects or entire collections. It loads a test file, builds nested collections (`1` → `sub-zero` and `scorpion`), links objects (`T.3b`, `T.3c`, `T.3d`), performs unlink operations based on a parameter (none, object‑only, or collection removal), saves the modified scene, dumps the scene structure to JSON, and compares it against a reference JSON file. Three test methods cover: linking only, linking and unlinking an object, and linking and removing a collection.
## Objective
Ensure that the dependency graph and collection hierarchy remain consistent after scene collection operations (creation, object linking, unlinking, collection removal). The test verifies that the saved `.blend` file reflects the expected structure and that the JSON output matches the reference data.
## Scope
### In Scope
- Loading a test file (`layers.blend`) with predefined collections and objects (`T.3b`, `T.3c`, `T.3d`).
    
- Creating nested collections (`sub-zero`, `scorpion` under collection `1`).
    
- Linking objects: `T.3b` to `sub-zero`, `T.3c` and `T.3d` to `scorpion`.
    
- Three modes:
    
    - `NONE`: link objects only (no unlink).
        
    - `OBJECT`: link `T.3d` to `scorpion`, then unlink it from `scorpion`.
        
    - `COLLECTION`: link `T.3d` to `scorpion`, then remove both `sub-zero` and `scorpion` from the master collection.
        
- Saving the modified scene to a temporary `.blend` file.
    
- Querying scene collections and layers, dumping to JSON.
    
- Comparing JSON output against reference files (`layers_nested.json`, `layers.json`).
### Out of Scope
-  View layer visibility, selectability, or other flags.
    
- Performance or memory.
    
- UI interactions.
    
- File system errors (temporary directory assumed writable).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/layer_syncing.py`
## Approach
The test uses the custom `ViewLayerTesting` base class. A helper method `do_syncing` handles the common setup: loading the test file, building collections, linking objects, performing the specified unlink operation, saving to a temporary file, querying the scene structure, dumping to JSON, and comparing against a reference JSON file using `compare_files()`. The test runs headlessly via Blender's background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_layer_syncing.py`
