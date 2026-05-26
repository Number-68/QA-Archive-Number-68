# Test Plan: View Layer – Collection Movement and Automatic Renaming

## Description
This test validates that when a collection is moved within the view layer hierarchy (above, below, or into another collection), duplicate names among sibling collections are automatically resolved. It creates a scenario where a sub‑collection initially has the same name as an existing collection (after creation), then moves that sub‑collection to become a sibling of the existing collection, triggering a forced rename. The test checks that after the move, the two collections no longer have identical names.
## Objective
Ensure that moving a collection (`move_above`, `move_below`, `move_into`) enforces the rule that sibling collections must have unique names. Specifically, when a collection with a duplicate name becomes a sibling of the collection it duplicated, the system automatically renames either the moved collection or the existing one so that their names differ.
## Scope
### In Scope
- Creating a hierarchy with a master collection, an existing collection (`one`), a new sibling (`two`), and a sub‑collection of `two` initially sharing the name of `one`.
    
- Moving the sub‑collection above `one` (`move_above`) – verifies names become different.
    
- Moving the sub‑collection below `one` (`move_below`) – verifies names become different.
    
- Moving the sub‑collection into the master collection (`move_into`) – verifies names become different.
### Out of Scope
- Moving collections that already have unique names (no rename needed).
    
- Undo/redo operations.
    
- Performance or memory.
    
- UI interactions (operators called directly).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_collection_rename_b.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). A `setup_collections()` helper builds the initial hierarchy and returns a lookup dictionary. Each test method calls the appropriate move operation on the sub‑collection, then asserts that the names of the previously duplicate collections are no longer equal. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_collection_rename_b.py`
