# Test Plan: View Layer – Object Visibility When Both Parent and Child Collections Are Disabled

## Description
This test validates that an object linked to a child collection becomes invisible when both the parent collection (directly disabled) and the child collection (disabled via both the parent's nested reference and its own direct reference) are disabled. It creates a hierarchy (`Mom` containing `Kid`), links the cube to `Kid`, links both collections to a fresh view layer, enables `Mom`, then disables `Kid` through `layer_collection_mom.collections[...].enabled` and also disables `layer_collection_kid.enabled`, updates the dependency graph, and checks that the cube is invisible.
## Objective
Ensure that an object is hidden when all collections containing it are disabled. The test verifies that disabling a child collection via two separate handles (nested reference and direct link) both propagate the disabled state, and that the object becomes invisible (logical AND across multiple disabled references).
## Scope
### In Scope
- Creating a new view layer, removing default collection, and making it active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both `Mom` and `Kid` collections to the view layer.
    
- Enabling `Mom`.
    
- Disabling the child collection via both `layer_collection_mom.collections[layer_collection_kid.name].enabled = False` and `layer_collection_kid.enabled = False`.
    
- Updating the dependency graph.
    
- Asserting the cube is not visible (`cube.visible_get()`).
### Out of Scope
- Selectability or other flags.
    
- Other enable/disable combinations.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_b.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a cube, builds a parent‑child collection hierarchy, links both to a view layer, enables the parent, disables the child through both references, updates the depsgraph, and asserts invisibility. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_b.py`
