# Test Plan: View Layer – Background Set Linking and Depsgraph Integration
## Description
This test validates that a scene can be set as a **background set** for another scene, linking its objects into the main scene's dependency graph. It verifies that objects from the background set are correctly added to the depsgraph, flagged as `is_from_set`, and removed when the background set is cleared. It also checks that object relationships (parenting) within the background set are preserved.
## Objective
Ensure that `main_scene.background_set = background_scene` properly links all objects from the background scene into the main scene's depsgraph, that each object is marked as originating from a set (`is_from_set`), and that setting `background_set = None` removes those objects from the depsgraph without leaving leftovers.
## Scope
### In Scope
- Creating a new main scene and setting an existing scene as background set.
    
- Verifying depsgraph object count before and after linking.
    
- Checking `is_from_set` flag on all depsgraph objects.
    
- Testing that parent relationships within the background set are reflected in depsgraph (indirectly via object count).
    
- Clearing the background set and confirming depsgraph returns to empty state.
### Out of Scope
- Overrides or local modifications of background set objects.
    
- Rendering or viewport visibility.
    
- Performance or memory usage.
    
- Multiple background sets or nesting.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_background_set.py`
## Approach
The script uses a custom test base class `ViewLayerTesting` (which extends `unittest.TestCase`). It creates a new scene, sets an existing scene as background, forces depsgraph updates, and asserts object counts and `is_from_set` flags. It then clears the background set and confirms depsgraph is empty. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_background_set.py`
