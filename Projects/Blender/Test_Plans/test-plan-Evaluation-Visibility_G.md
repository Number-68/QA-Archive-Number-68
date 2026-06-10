# Test Plan: View Layer – Visibility of Newly Added Empty Object

## Description
This test verifies that when an empty object is added to the scene, it becomes visible in the view layer's dependency graph. It relies on a helper method `do_visibility_object_add('EMPTY')` from the `ViewLayerTesting` base class, which likely creates an empty object, links it to the view layer, updates the depsgraph, and checks its visibility.
## Objective
Ensure that an empty object added to the scene is correctly evaluated as visible by the dependency graph after an update. This confirms that basic object addition and depsgraph integration work for empty object types.
## Scope
### In Scope
- Adding an empty object to the scene.
    
- Updating the dependency graph.
    
- Checking that the object is visible (`visible_get()`).
    
- The helper method abstracts the steps (creation, linking, depsgraph update, assertion).
### Out of Scope
- Other object types (meshes, cameras, etc.) – only `'EMPTY'` is tested.
    
- Selectability, hiding, or other flags.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_g.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It calls `do_visibility_object_add('EMPTY')`, which internally creates an empty object, adds it to the view layer, updates the dependency graph, and asserts visibility. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_g.py`
