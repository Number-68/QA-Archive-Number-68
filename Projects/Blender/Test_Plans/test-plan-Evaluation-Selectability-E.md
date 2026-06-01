# Test Plan: View Layer – Object Visibility with Parent Enabled, Child Disabled and Unselectable

## Description
This test validates that when a parent collection is enabled, but its child collection is both disabled and marked unselectable, objects in the child collection remain visible yet become unselectable. It creates a custom view layer, builds a hierarchy (`Mom` containing `Kid`), links both collections to the view layer, links a cube to `Kid`, enables `Mom`, selects the cube, sets `Kid` as unselectable via the parent's nested reference, then disables `Kid` entirely, updates the dependency graph, and checks that the cube is still visible but not selected.
## Objective
Ensure that disabling a child collection (`enabled = False`) and marking it unselectable (`selectable = False`) does not hide the objects (they remain visible because the parent is enabled), but they become unselectable (the selection state is cleared or cannot be set). The test confirms that the dependency graph correctly combines enable/selectable flags: visibility propagates from the enabled parent, while selectability is determined by the child's own flags.
## Scope
### In Scope
- Creating a new view layer, removing default collection, and making it active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both `Mom` and `Kid` to the view layer.
    
- Enabling `Mom` (`enabled = True`).
    
- Selecting the cube.
    
- Setting `layer_collection_mom.collections[layer_collection_kid.name].selectable = False`.
    
- Setting `layer_collection_kid.enabled = False`.
    
- Updating the dependency graph.
    
- Asserting cube is visible (`cube.visible_get()`) but not selected (`cube.select_get()`).
### Out of Scope
- Other combinations of enable/selectable flags.
    
- Performance or memory.
    
- UI interactions or other layer settings.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_selectability_e.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a scene, cube, and view layer, builds the collection hierarchy, links collections, enables the parent, selects the cube, sets the child as unselectable and disabled, updates the depsgraph, and asserts visibility and selection state. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_selectability_e.py`
