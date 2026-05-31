# Test Plan: View Layer – Object Selectability with Disabled Child Collection Selectability

## Description
This test validates that when a child collection's `selectable` property is set to `False` via its parent collection's reference, objects within that child collection remain selectable if the collection itself is still enabled. It creates a custom view layer, builds a hierarchy of collections (`Mom` containing `Kid`), links both collections to the view layer, links a cube to the child collection, ensures the parent collection is enabled, updates the dependency graph, selects the cube, then sets the child collection's `selectable` to `False` through the parent's nested reference, updates again, and verifies that the cube remains both visible and selected.
## Objective
Ensure that setting `selectable = False` on a collection does not affect the selection state or visibility of objects already selected, and that the object remains selectable if the collection is still enabled. The test checks that the dependency graph correctly updates selection and visibility flags when the `selectable` property changes, and that the cube stays selected and visible.
## Scope
### In Scope
- Creating a new view layer, removing its default collection, and setting it as active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both `Mom` and `Kid` collections to the view layer.
    
- Enabling the parent collection.
    
- Updating the dependency graph.
    
- Selecting the cube.
    
- Setting `layer_collection_mom.collections[layer_collection_kid.name].selectable = False`.
    
- Updating the dependency graph again.
    
- Asserting that the cube is still visible and still selected.
### Out of Scope
- Testing the effect of `selectable` on new selections after it is disabled.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_selectability_d.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a new scene, a cube, a fresh view layer, builds a collection hierarchy, links collections, enables the parent collection, updates the dependency graph, selects the cube, and asserts visibility and selection. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_selectability_d.py`
