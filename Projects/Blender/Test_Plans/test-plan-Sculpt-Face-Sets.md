# Test Plan: Sculpt Face Sets – Visibility Toggle and Hide/Show Validation
## Description
This test validates the `sculpt.face_set_change_visibility` operator in Blender, which controls the visibility of faces in sculpt mode based on face sets. It tests three visibility modes: `TOGGLE` (inverts visibility of all faces if no active face set, or hides non‑active face sets and restores on second toggle), `HIDE_ACTIVE` (hides the specified active face set), and `SHOW_ACTIVE` (shows the specified active face set). The test loads a prepared mesh (`30k_monkey_mask_and_face_set.blend`) containing pre‑assigned face sets, toggles sculpt mode, applies visibility changes, and verifies the state of the `.hide_poly` attribute for each face.
## Objective
Ensure that face set visibility operations correctly hide and show faces according to the mode and active face set. Specifically: toggling with no face set hides all faces on first call and reveals all on second call; toggling with an active face set hides all faces not in that set on first call and reveals all on second call; `HIDE_ACTIVE` hides only faces belonging to the specified set; `SHOW_ACTIVE` reveals those faces without affecting others. All operations should leave the mesh in a consistent hidden/unhidden state.
## Scope
### In Scope
- `TOGGLE` mode with `active_face_set=0` – hides all faces, then reveals all.
    
- `TOGGLE` mode with `active_face_set=2` – hides all faces not in face set 2, then reveals all.
    
- `HIDE_ACTIVE` mode with `active_face_set=2` – hides only faces in face set 2.
    
- `SHOW_ACTIVE` mode with `active_face_set=2` – reveals faces in face set 2 (making them visible again).
    
- Use of a test mesh containing face set data (`.sculpt_face_set` attribute).
    
- Verification via reading the `.hide_poly` attribute (face‑wise boolean mask).
    
- Counting hidden faces with `numpy` and comparing against expected counts.
### Out of Scope
- Other visibility modes (`HIDE_OTHER`, `SHOW_ALL`, etc.).
    
- Face set creation, deletion, or editing.
    
- Behavior on meshes without face sets.
    
- Performance or memory usage.
    
- UI or interaction aspects of face set visibility.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/face_set_test.py`
## Approach
Automated execution using the provided Python script, which relies on the `unittest` framework within Blender. The test loads a prepared `.blend` file (`30k_monkey_mask_and_face_set.blend`) containing a mesh with defined face sets. It enters sculpt mode and, for each test method, calls `sculpt.face_set_change_visibility` with the appropriate mode and active face set. Before and after each operation, helper function `get_attribute_data` reads the `.hide_poly` attribute (face domain) into a `numpy` boolean array. The test asserts that the number of hidden faces matches the expected count (e.g., all faces, no faces, faces in a specific set, or faces not in a specific set). The script is intended to be run headlessly via Blender’s background mode with the `--testdir` argument pointing to the directory containing the test `.blend` file.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/face_set_test.py`
