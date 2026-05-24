# Test Plan: View Layer – Collection Syncing and Outliner Collection Creation

## Description
This test validates that when a new view layer is created, it correctly inherits the master collection and its child collections. It then checks that adding a new collection via the outliner operator (`bpy.ops.outliner.collection_new()`) properly updates the view layer's collection hierarchy, with the new collection appearing alongside existing ones.
## Objective
Ensure that a fresh view layer contains the master collection as its first collection, with exactly one child collection (`"Collection 1"`). After invoking `outliner.collection_new()`, verify that the operator succeeds and the view layer's first collection now has two children (`"Collection 1"` and `"Collection 2"`), confirming that view layer syncing with the outliner works correctly.
## Scope
### In Scope
- Creating a new view layer named `"All"`.
    
- Checking that the view layer initially has one collection (the master collection).
    
- Verifying that the master collection's children include `"Collection 1"`.
    
- Calling `bpy.ops.outliner.collection_new()` and confirming it returns `{'FINISHED'}`.
    
- Checking that after the operator, the view layer's first collection now contains `"Collection 1"` and `"Collection 2"`.
### Out of Scope
- Deleting or renaming collections.
    
- Nested collections beyond one level.
    
- Performance or memory usage.
    
- UI interactions (operator is called directly).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_collection_new_sync.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a new view layer, inspects its collection hierarchy using `view_layer.collections[0].collections`, then executes `bpy.ops.outliner.collection_new()` and re‑checks the set of child collection names. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_collection_new_sync.py`
