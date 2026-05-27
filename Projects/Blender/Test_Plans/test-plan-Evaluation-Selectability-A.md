# Test Plan: View Layer – Object Selectability with Disabled Parent Collection

## Description
This test validates that an object remains selectable and visible when its collection is linked directly to a view layer, even if the parent collection (in the scene's master hierarchy) is disabled in that same view layer. It creates a custom view layer, builds a collection hierarchy (`Mom` containing `Kid`), links both collections to the view layer, disables the parent (`Mom`), and then checks that an object in the child collection (`Kid`) is still selectable and visible.
## Objective
Ensure that disabling a collection in a view layer does not affect the visibility or selectability of objects belonging to a child collection that is also directly linked to the same view layer. The test verifies that the dependency graph correctly evaluates collection enable states when collections are linked at multiple levels.
## Scope
### In Scope
- Creating a new view layer and removing its default collection.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to `Kid`.
    
- Linking both `Mom` and `Kid` collections to the view layer.
    
- Disabling `Mom` in the view layer.
    
- Forcing a depsgraph update.
    
- Checking that the cube is visible (`cube.visible_get()`) and selected (`cube.select_get()`).
### Out of Scope
- Other collection enable/disable combinations.
    
- Overrides or local collections.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_selectability_a.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a new scene with a cube, builds the collection hierarchy, links collections to a fresh view layer, disables the parent collection, updates the depsgraph, sets selection on the cube, and asserts visibility and selection state. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_selectability_a.py`
