# Test Plan: View Layer – Object Visibility When Ancestor Collection Is Disabled

## Description
This test validates that an object linked to a nested collection becomes invisible when the top‑level ancestor collection (master collection) is disabled in the view layer. It clears the scene, builds a hierarchy (`parent` → `child linked`), links an empty object to the child, links the master collection to a fresh view layer, verifies the object is visible when the ancestor collection is enabled, then disables the ancestor and confirms the object becomes invisible.
## Objective
Ensure that disabling a collection in the view layer propagates to all descendant collections, and that any object in those descendants becomes invisible. The test verifies that the dependency graph correctly evaluates the enabled state of the entire hierarchy.
## Scope
### In Scope
- Clearing all default objects and collections from the scene.
    
- Creating a parent collection and a nested child collection.
    
- Adding an empty object to the child collection.
    
- Linking the master collection (which contains the parent and child) to the view layer.
    
- Enabling the master collection link (default true).
    
- Updating the dependency graph and checking object visibility.
    
- Disabling the master collection link and updating again.
    
- Asserting object becomes invisible.
### Out of Scope
- Other link‑specific flags (selectable, hide, etc.).
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_j.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It manually clears the scene, builds a collection hierarchy, links the master collection to the view layer, updates depsgraph, checks visibility, disables the link, updates again, and asserts invisibility. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_j.py`
