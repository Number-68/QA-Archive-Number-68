# Test Plan: View Layer – Group Instance Object Deletion Stability

## Description
This test validates that deleting the original object of a group instance does not crash or cause errors. It creates a group containing a mesh object, instances that group via an empty object (with `instance_type = 'GROUP'`), selects the original object, and deletes it using `bpy.ops.object.delete()`. The test passes if no exceptions are raised during deletion and the depsgraph updates cleanly.
## Objective
Ensure that deleting the source object of a group instance is safe, and that the dependency graph handles removal of an object that is still referenced by a group instance without crashing. This verifies that group instance references are properly managed when the original object is deleted.
## Scope
### In Scope
- Cleaning up the scene to isolate the test.
    
- Creating a group (`Switch`) and linking the active object to it.
    
- Creating an empty object that instances the group.
    
- Selecting the original object and setting it as active.
    
- Updating the dependency graph.
    
- Deleting the original object via `bpy.ops.object.delete()`.
    
- Test passes if no crashes or errors occur.
### Out of Scope
- Verifying the group or instance state after deletion.
    
- Checking object counts or selection states.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_group_e.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It cleans up extra objects and view layers, creates a group with the active object, instances the group via an empty, selects and deletes the original object, and updates the dependency graph. The test runs headlessly via Blender's background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_group_e.py`
