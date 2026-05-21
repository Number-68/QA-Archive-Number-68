# Test Plan: View Layer – Active Collection Index Navigation
## Description
This test validates that the `active_index` property of `layer.collections` correctly navigates nested collections in a view layer. It loads a test `.blend` file with pre‑defined collections, creates new nested collections, links them to a fresh view layer, and then checks that setting `active_index` from 0 to 9 selects the expected collection by name.
## Objective
Ensure that `layer.collections.active_index` works recursively across nested collections, and that `layer.collections.active.name` returns the correct collection name after each index change. This is critical for Outliner UI and view layer management.
## Scope
### In Scope
- Loading `layers.blend` (contains collections `1` through `5` and objects `T.3b`, `T.3c`).
    
- Creating nested collections (`sub-zero` under `1`, `scorpion` under `sub-zero`).
    
- Creating a new view layer and linking `sub-zero` into it.
    
- Iterating `active_index` through 10 values and verifying the active collection name against a hardcoded list.
    
- Test uses custom `ViewLayerTesting` base class (provides `get_root()`, `rename_collections()`).
### Out of Scope
- Other view layer properties (exclude, hide, etc.).
    
- Performance or memory.
    
- UI interaction – only the API is tested.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_active_collection.py`
## Approach
The script uses `unittest` with a custom base class `ViewLayerTesting`. It opens a test `.blend` file, creates nested collections and a new view layer, then loops over indices 0–9, setting `layer.collections.active_index` each time and asserting that `layer.collections.active.name` matches an expected list of collection names (including duplicates for nested paths). The test runs headlessly with Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_active_collection.py`
