# Test Plan: View Layer – Collection Linking, Object Selectability After Link

## Description
This test validates that when a new collection is linked to a view layer, it is enabled and selectable by default, and that an object added to the source scene collection becomes selectable after a depsgraph update. It creates a new scene collection, links it to the view layer, adds a cube to the scene collection, updates the dependency graph, selects the cube, and checks that the cube's selection state is true.
## Objective
Ensure that a newly linked collection in a view layer has `enabled` and `selectable` both set to `True`, and that an object added to the corresponding scene collection is selectable after a depsgraph update. The test verifies that the dependency graph correctly propagates the collection's default flags and allows object selection.
## Scope
### In Scope
- Creating a new collection in the scene's master collection.
    
- Linking that collection to the active view layer.
    
- Adding a cube object to the scene collection.
    
- Updating the dependency graph.
    
- Verifying the linked collection is enabled and selectable.
    
- Selecting the cube and confirming it returns `True` for `select_get()`.
### Out of Scope
- Disabling collections or testing visibility.
    
- Multiple view layers or nested collections.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_selectability_f.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a new scene collection, links it to the view layer, adds a cube to the collection, updates the dependency graph, checks the collection's `enabled` and `selectable` flags, selects the cube, and asserts the cube's selection state. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_selectability_f.py`
