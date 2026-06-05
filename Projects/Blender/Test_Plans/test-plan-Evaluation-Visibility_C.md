# Test Plan: View Layer – Object Visibility When Child Collection Enabled Despite Parent’s Nested Disable

## Description
This test validates that an object linked to a child collection remains visible when the parent collection is enabled, the child collection is disabled through the parent’s nested reference, but the child collection itself is explicitly enabled via its direct link. It creates a hierarchy (`Mom` containing `Kid`), links the cube to `Kid`, links both collections to a fresh view layer, enables `Mom`, disables `Kid` through `layer_collection_mom.collections[...].enabled = False`, but then directly sets `layer_collection_kid.enabled = True`, updates the dependency graph, and checks that the cube is visible.
## Objective
Ensure that an object is visible if **any** of its collection references (including the direct link) are enabled, even when a different reference to the same collection is disabled. The test verifies that the dependency graph treats the child collection as enabled because the direct `enabled` flag overrides the nested reference’s disable state.
## Scope
### In Scope
- Creating a new view layer, removing default collection, and making it active.
    
- Building a scene collection hierarchy: `Mom` (parent) and `Kid` (child).
    
- Adding a cube to the `Kid` collection.
    
- Linking both `Mom` and `Kid` collections to the view layer.
    
- Enabling `Mom`.
    
- Disabling the child collection via `layer_collection_mom.collections[layer_collection_kid.name].enabled = False`.
    
- Explicitly enabling the child collection via `layer_collection_kid.enabled = True`.
    
- Updating the dependency graph.
    
- Asserting the cube is visible (`cube.visible_get()`).
### Out of Scope
- Other enable/disable combinations.
    
- Selectability or visibility flags besides `enabled`.
    
- Performance or memory.
    
- UI interactions.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_evaluation_visibility_c.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It creates a cube, builds a parent‑child collection hierarchy, links both collections to a view layer, enables the parent, sets the child as disabled via the parent’s nested reference but re‑enables it directly, updates the depsgraph, and asserts visibility. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_evaluation_visibility_c.py`
