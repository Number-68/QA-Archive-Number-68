# Test Plan: View Layer – Object Visibility and Selectability with Enabled Parent Collection

## Description
This test validates that when a parent collection is enabled in a view layer, an object in a child collection remains visible and selectable. It creates a custom view layer, builds a hierarchy of collections (`Mom` containing `Kid`), links both collections to the view layer, links a cube to the child collection, ensures the parent collection is enabled, updates the dependency graph, selects the cube, and verifies that the cube is both visible and selected.
## Objective
Ensure that enabling a parent collection in a view layer correctly allows objects in child collections to be visible and selectable. The test verifies that the dependency graph propagates the enabled state to the child collection, making the cube visible and selectable after an update.
## Scope
### In Scope
- Creating a new view layer, removing its default collection, and setting it as active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both `Mom` and `Kid` collections to the view layer.
    
- Ensuring `layer_collection_mom.enabled = True`.
    
- Updating the dependency graph.
    
- Setting the cube as selected.
    
- Checking that the cube is visible (`cube.visible_get()`) and selected (`cube.select_get()`).
### Out of Scope
- Disabling collections or testing invisibility.
    
- Performance or memory.
    
- UI interactions or other modifier/layer settings.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_selectability_b.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a new scene, a cube, a fresh view layer, builds a collection hierarchy, links collections, enables the parent collection, updates the dependency graph, selects the cube, and asserts visibility and selection. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_selectability_b.py`
