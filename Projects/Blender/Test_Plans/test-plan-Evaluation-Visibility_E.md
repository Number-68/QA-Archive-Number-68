# Test Plan: View Layer – Object Visibility When Linked to Multiple Collections (One Enabled, One Disabled)

## Description
This test validates that an object linked to two scene collections, both linked to the view layer, remains visible as long as at least one of those collections is enabled. It creates two scene collections (`Mom` and `Kid`), links the same cube to both, links both collections to a fresh view layer, enables `Mom`, disables `Kid`, updates the dependency graph, and checks that the cube is visible.
## Objective
Ensure that an object's visibility in a view layer is determined by the logical OR of its parent collections' enabled states. The test verifies that disabling one collection while keeping another enabled does not hide the object, and that the dependency graph correctly propagates the enabled state from at least one collection to make the object visible.
## Scope
### In Scope
- Creating a new view layer, removing its default collection, and making it active.
    
- Building two scene collections (`Mom` and `Kid`).
    
- Linking the same cube object to both scene collections.
    
- Linking both collections to the view layer.
    
- Enabling `Mom` (`enabled = True`) and disabling `Kid` (`enabled = False`).
    
- Updating the dependency graph.
    
- Asserting that the cube is visible (`cube.visible_get()`).
### Out of Scope
- Both collections disabled (would hide object).
    
- Selectability or other flags.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_e.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a cube, two scene collections, links the cube to both, links the collections to a view layer, sets one enabled and the other disabled, updates the depsgraph, and asserts visibility. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_e.py`
