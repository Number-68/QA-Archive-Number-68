# Test Plan: View Layer – Object Visibility When Linked to Enabled Parent Collection

## Description
This test validates that an object linked directly to an enabled parent collection remains visible even when the child collection (which also contains the object) is disabled via both a nested reference and its direct link. It creates a hierarchy (`Mom` containing `Kid`), links the cube to both `Mom` and `Kid`, links both collections to a fresh view layer, enables `Mom`, disables `Kid` through both `layer_collection_mom.collections[...].enabled` and `layer_collection_kid.enabled`, updates the dependency graph, and checks that the cube is visible.
## Objective
Ensure that an object’s visibility is determined by any collection that directly contains it, regardless of the enabled state of descendant or other collections that also contain it. The test verifies that because the cube is directly linked to the enabled `Mom` collection, it remains visible even though the child `Kid` collection (which also contains the cube) is disabled.
## Scope
### In Scope
- Creating a new view layer, removing its default collection, and making it active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Linking the same cube object to both `Mom` and `Kid` collections.
    
- Linking both collections to the view layer.
    
- Enabling `Mom` (`enabled = True`).
    
- Disabling the child collection via both the parent’s nested reference and its own direct link.
    
- Updating the dependency graph.
    
- Asserting that the cube is visible (`cube.visible_get()`).
### Out of Scope
- Cases where the cube is only linked to the disabled child collection.
    
- Selectability or other flags.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_f.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a cube, builds a parent‑child collection hierarchy, links the cube to both collections, links the collections to a view layer, enables the parent, disables the child through both references, updates the depsgraph, and asserts visibility. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_f.py`
