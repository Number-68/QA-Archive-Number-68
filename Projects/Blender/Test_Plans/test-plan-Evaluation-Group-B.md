# Test Plan: View Layer – Group Creation Preserves Collection Visibility Flags

## Description
This test validates that when a group is created from a view layer collection, the enabled and selectable flags of the original collections are correctly preserved in the newly created group's view layer. It builds a nested collection hierarchy (grandma → mom), sets specific enabled/selectable states, creates a group from the top-level collection, and asserts that the group's view layer collections inherit the same flags.
## Objective
Ensure that `layer_collection.create_group()` correctly copies the enabled and selectable states of the original collection hierarchy into the new group's view layer, and that the flags are not lost or reset during group creation.
## Scope
### In Scope
- Creating a nested collection hierarchy (`grandma` → `mom`).
    
- Linking an object (`Child`) to the `mom` collection.
    
- Linking the top-level collection (`grandma`) to the view layer.
    
- Setting `grandma.enabled = True`, `mom.enabled = False`, `mom.selectable = True`.
    
- Updating the dependency graph.
    
- Creating a group from `grandma_layer_collection`.
    
- Updating the dependency graph again.
    
- Checking that the group's view layer has one collection (`grandma_group_layer`) with `enabled=True`, `selectable=True`.
    
- Checking that the child collection (`mom_group_layer`) has `enabled=False`, `selectable=True`.
### Out of Scope
- Other group operations (linking, unlinking, deleting).
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_group_b.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It cleans the scene, builds a collection hierarchy, sets flags, creates a group, updates the depsgraph, and asserts flag values on the group's view layer collections. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_group_b.py`
