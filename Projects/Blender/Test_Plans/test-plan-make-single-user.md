# Test Plan: View Layer – Make Single User Stability Test

## Description
This test validates that the `bpy.ops.object.make_single_user` operator can be called without crashing on a minimal scene. It cleans up extra objects and view layers, unlinks all collections from the master collection, links the master collection back, selects the active object, updates the dependency graph, and then invokes `make_single_user(object=True)`. The test passes if no exceptions are raised during execution.
## Objective
Ensure that the `make_single_user` operator works on a clean, minimal scene without crashing or causing errors. This serves as a basic stability smoke test for the operator, verifying that it can handle a scene with a single object and no extraneous collections.
## Scope
### In Scope
- Cleaning up extra objects (keeping only the active object).
    
- Removing extra view layers (keeping only the active one).
    
- Removing all collections from the master collection.
    
- Linking the master collection back to the view layer.
    
- Selecting the active object.
    
- Updating the dependency graph.
    
- Calling `bpy.ops.object.make_single_user(object=True)`.
    
- Test passes if no crashes or errors occur.
### Out of Scope
- Verifying the result of `make_single_user` (e.g., checking if objects were duplicated or users changed).
    
- Testing other modes or parameters of the operator.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_make_single_user.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It cleans up the scene to a minimal state, selects the active object, updates the depsgraph, and calls `bpy.ops.object.make_single_user(object=True)`. The test runs headlessly via Blender's background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_make_single_user.py`
