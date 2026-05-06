# Test Plan: Brush Asset Activation and Local Saving Tests
## Description
This test validates that Blender can activate brush assets from the built‑in Essentials asset library and save local copies of them. It verifies the `brush.asset_activate` operator for brushes like Draw, Smooth, and Mask, including the toggle behavior (activating a brush when it is already active reverts to the previously used brush). It also checks that `brush.asset_save_as` creates a local copy of the brush with the correct brush type and reference count.
## Objective
Ensure that brush asset activation reliably loads the correct brush from the Essentials library, that the `use_toggle` parameter toggles between the current and previous brush as expected, and that saving an asset locally produces a usable copy that preserves the brush type and has at least one user.
## Scope
### In Scope
- Activating brushes from the Essentials asset library (Draw, Smooth, Mask) via `brush.asset_activate`.
    
- Toggle behavior: when `use_toggle=True` and the brush differs, the specified brush becomes active; when the same brush is toggled again, the previously active brush is restored.
    
- Saving the active brush as a local asset via `brush.asset_save_as` with a new name, local library reference, and empty catalog path.
    
- Verification that the saved brush exists in `bpy.data.brushes`, has the correct `sculpt_brush_type` (e.g., 'SMOOTH'), and has at least one user.
### Out of Scope
- Other asset libraries (only Essentials is tested).
    
- Non‑brush assets (materials, node groups, etc.).
    
- Sculpt mode settings beyond brush activation.
    
- File system errors or disk space issues during save.
    
- Performance or memory usage.
    
- UI interactions beyond the operators.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/brush_asset_test.py`
## Approach
Automated execution using the provided Python script, which relies on the `unittest` framework within Blender. The script enters sculpt mode, then activates a brush asset using `brush.asset_activate` with appropriate `asset_library_type` and `relative_asset_identifier`. Assertions check the returned operator result and the name of the currently active brush. Toggle tests activate a brush twice with `use_toggle=True` and verify that after the second call the brush reverts to the previous one. For saving, the script activates the Smooth brush, calls `brush.asset_save_as` to create a local copy, then checks that the copy exists in `bpy.data.brushes`, has the expected type, and has at least one user. The script is intended to be run headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/brush_asset_test.py`
