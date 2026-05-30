# Test Plan: View Layer – Object Visibility and Selectability with Disabled Collections

## Description
This test validates that when a collection containing an object is disabled in a view layer, the object becomes invisible and unselectable. It creates a custom view layer, builds a hierarchy of collections (`Mom` containing `Kid`), links both collections to the view layer, links a cube to the child collection, then disables the child collection (both via its direct entry and through the parent's reference). After a depsgraph update, the test checks that the cube is neither visible nor selectable.
## Objective
Ensure that disabling a collection in a view layer correctly hides and prevents selection of all objects within that collection, even when the collection has been linked multiple times (once directly, once as a child of another linked collection). The test verifies that the dependency graph propagates the disabled state to the object, making it invisible and unselectable.
## Scope
### In Scope
- Creating a new view layer, removing its default collection, and setting it as active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both `Mom` and `Kid` collections to the view layer.
    
- Disabling the child collection via both `layer_collection_mom.collections[layer_collection_kid.name]` and `layer_collection_kid.enabled`.
    
- Forcing a depsgraph update.
    
- Checking that the cube is not visible (`cube.visible_get()`) and not selectable (`cube.select_get()`).
### Out of Scope
- Partial disabling (e.g., only one of the two references).
    
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
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a new scene, a cube, a fresh view layer, builds a collection hierarchy, links collections, disables the child collection through both available handles, updates the dependency graph, and asserts that the cube is invisible and unselected. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_selectability_b.py`
