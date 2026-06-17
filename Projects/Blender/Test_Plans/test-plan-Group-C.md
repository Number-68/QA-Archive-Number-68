# Test Plan: View Layer – Group Creation from a Non-Directly Linked Collection

## Description
This test validates that creating a group from a collection that is not directly linked to the view layer (but is nested inside another collection that is linked) completes without crashing. It builds a hierarchy (`Grand-Mother` → `Mother`), links child objects to both, links only the top‑level collection to the view layer, then creates a group from the nested `Mother` collection, updating the dependency graph before and after. The test passes if no exceptions are raised.
## Objective
Ensure that `layer_collection.create_group()` can be called on a nested collection that is not directly linked to the view layer, and that the depsgraph update after group creation does not cause errors. This verifies that group creation correctly handles intermediate collection links.
## Scope
### In Scope
- Cleaning the scene (removing default objects/collections).
    
- Creating three empty objects (`Child`).
    
- Building a collection hierarchy: `Grand-Mother` containing `Mother`.
    
- Linking `Child[0]` to `Grand-Mother`, `Child[1]` to `Mother`.
    
- Linking only `Grand-Mother` to the view layer.
    
- Retrieving the nested `Mother` layer collection via `grandma_layer_collection.collections[...]`.
    
- Creating a group from `mom_layer_collection`.
    
- Updating the dependency graph after group creation.
    
- No assertions – test passes if no crashes occur.
### Out of Scope
- Verifying group structure or flags.
    
- Checking object visibility or selection.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_group_c.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It cleans the scene, builds a nested collection hierarchy, links only the top‑level collection, retrieves the nested collection, creates a group, and updates the dependency graph. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_group_c.py`
