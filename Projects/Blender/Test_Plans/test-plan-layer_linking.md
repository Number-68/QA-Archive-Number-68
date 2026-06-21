# Test Plan: View Layer – Collection Linking, Unlinking, and Syncing with JSON Comparison

## Description
This test validates that creating a new view layer, linking and unlinking scene collections, and saving/loading `.blend` files correctly updates the dependency graph and scene structure. It builds a nested collection hierarchy (`1` → `sub-zero` → `scorpion`), links objects to both child collections, creates a new view layer, performs collection link/unlink operations based on a parameter, saves the file, dumps the scene structure to JSON, and compares it against a reference JSON file. Three test methods cover: creating a new layer only, linking a collection, and unlinking a collection.
## Objective
Ensure that view layer operations (new layer creation, collection linking, collection unlinking) produce a consistent scene structure that matches expected reference data. The test verifies that the dependency graph and collection hierarchy are correctly updated after these operations, and that the saved `.blend` file reflects the changes accurately.
## Scope
### In Scope
- Loading a test file (`layers.blend`) with predefined collections and objects.
    
- Creating nested collections (`sub-zero` under `1`, `scorpion` under `sub-zero`).
    
- Linking objects (`T.3b` to `sub-zero`, `T.3c` to `scorpion`).
    
- Creating a new view layer (`Fresh new Layer`).
    
- Three modes:
    
    - `LAYER_NEW`: create layer only (no link/unlink).
        
    - `COLLECTION_LINK`: link `sub-zero` to the new layer.
        
    - `COLLECTION_UNLINK`: link `sub-zero` then unlink the master collection.
        
- Saving the modified scene to a temporary `.blend` file.
    
- Querying scene collections and layers, dumping to JSON.
    
- Comparing JSON output against reference files (`layers_new_layer.json`, `layers_layer_collection_link.json`, `layers_layer_collection_unlink.json`).
### Out of Scope
- Other view layer operations (visibility, selectability, etc.).
    
- Performance or memory.
    
- UI interactions.
    
- File system errors (temporary directory assumed writable).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/layer_linking.py`
## Approach
The test uses the custom `ViewLayerTesting` base class. A helper method `do_layer_linking` handles the common setup: loading the test file, building collections, creating a new view layer, performing the specified link/unlink operation, saving to a temporary file, querying the scene structure, dumping to JSON, and comparing against a reference JSON file using `compare_files()`. The test runs headlessly via Blender's background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_layer_linking.py`
