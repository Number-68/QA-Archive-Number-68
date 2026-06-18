# Test Plan: View Layer – Group Save, Load, and Cleanup Persistence

## Description
This test validates that groups created from view layer collections are correctly saved to and loaded from `.blend` files, and that their user counts and object lists persist across save/load cycles. It creates a group from a layer collection, saves and reloads the file multiple times, verifies the group still has one user and three objects, empties the group of objects, saves and reloads again, verifies user count drops to zero and object list empties, and finally saves and reloads to confirm the group is removed entirely when no users remain.
## Objective
Ensure that group data (user count, linked objects) is correctly serialized to `.blend` files and restored on load. Verify that removing all objects from a group decreases its user count and that when the user count reaches zero, the group is automatically removed from `bpy.data.groups` on the next save/reload.
## Scope
### In Scope
- Creating a group from a layer collection.
    
- Checking initial group user count (1) and object count (3).
    
- Saving and reloading the `.blend` file 3 times, verifying persistence each time.
    
- Unlinking all objects from the group's collection.
    
- Saving and reloading: checking user count becomes 0 and object count becomes 0.
    
- Saving and reloading: checking that the group is completely removed from `bpy.data.groups`.
    
- Use of a temporary directory for saving files.
### Out of Scope
- Other group operations (linking, unlinking, visibility flags).
    
- Performance or memory.
    
- UI interactions.
    
- Error handling for file system issues.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_group_d.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). It cleans up extra view layers, creates a group, asserts initial state, then enters a loop of save/reload operations using a temporary directory. After verifying persistence, it empties the group, saves/reloads again, and checks user/object counts, then performs a final save/reload to confirm group removal. The test runs headlessly via Blender's background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_group_d.py`
