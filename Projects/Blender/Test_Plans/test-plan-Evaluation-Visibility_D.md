# Test Plan: View Layer – Object Selection Persistence After Setting Child Collection Unselectable

## Description
This test validates that an object remains selected after its collection is marked unselectable via a parent’s nested reference. It creates a cube linked to a child collection (`Kid`), links both parent (`Mom`) and child to a view layer, enables the parent, updates the dependency graph, selects the cube, then sets the child collection as unselectable through the parent’s reference, updates again, and checks that the cube is still visible and selected.
## Objective
Ensure that marking a collection as unselectable (`selectable = False`) does not clear the selection state of objects already selected, and that those objects remain selectable (i.e., `select_get()` returns `True`) and visible. The test verifies that the unselectable flag only prevents future selection changes but does not affect existing selection.
## Scope
### In Scope
- Creating a new view layer, removing default collection, and making it active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both collections to the view layer.
    
- Enabling the parent collection.
    
- Updating the dependency graph.
    
- Selecting the cube.
    
- Setting the child collection as unselectable via `layer_collection_mom.collections[layer_collection_kid.name].selectable = False`.
    
- Updating the dependency graph again.
    
- Asserting the cube is visible and selected.
### Out of Scope
- Attempting to select an object after the collection becomes unselectable (not tested).
    
- Other combinations of enable/selectable flags.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_d.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a cube, builds a parent‑child collection hierarchy, links collections to a view layer, enables the parent, updates depsgraph, selects the cube, sets the child collection unselectable, updates depsgraph, and asserts visibility and selection. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_d.py`
