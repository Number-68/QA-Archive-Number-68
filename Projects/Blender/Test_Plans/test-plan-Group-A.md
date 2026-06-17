# Test Plan: View Layer – Group Creation Stability Test

## Description
This test validates that creating a new group (via `layer_collection.create_group()`) does not crash Blender or cause any errors. It cleans up extra view layers, creates a group, and updates the dependency graph. No explicit assertions are made; the test passes if the operations complete without raising exceptions.
## Objective
Ensure that the `create_group` method on a view layer’s collection can be called safely without crashing the depsgraph or leaving the scene in an inconsistent state. This serves as a basic stability smoke test for group creation.
## Scope
### In Scope
- Removing all but the first view layer from the scene.
    
- Calling `layer_collection.create_group()` on the current layer collection.
    
- Updating the dependency graph.
    
- No crashes or errors during these steps.
### Out of Scope
- Verifying the structure or contents of the created group.
    
- Checking group visibility, selectability, or other properties.
    
- Performance or memory usage.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_group_a.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It cleans up extra view layers to isolate the test environment, then calls `layer_collection.create_group()` and `bpy.context.view_layer.update()`. The test passes if no exceptions are raised. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_group_a.py`
