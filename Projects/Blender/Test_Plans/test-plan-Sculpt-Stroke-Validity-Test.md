# Test Plan: Stroke Validity Across Brush Types and Backends

## Description
This test validates over 40 sculpt brushes (Draw, Clay, Smooth, Grab, Mask, Face Set, Paint, Cloth, etc.) on different mesh backends (regular, multires, color attribute, mask attribute). For each brush and backend, it performs one stroke at the mesh center and checks that vertex positions, mask values, face set IDs, or color channels remain finite (no NaN or infinite) and that at least one value changes – proving the brush actually does something.
## Objective
Ensure that every brush from the Essentials asset library can execute a stroke without producing invalid geometry (NaN/inf) and without silently failing (i.e., the mesh or attribute must change). This catches mathematical errors in brush code that could corrupt sculpt data.
## Scope
### In Scope
- All sculpt brushes listed in the file (Draw, Clay, Inflate, Smooth, Grab, Mask, Face Set Paint, Airbrush, Paint Blend, Cloth brushes, etc.) – over 40 total.
    
- Backends: regular mesh, multires, color attribute, mask attribute.
    
- Validation of position, mask, face set, and color attributes.
    
- Normal and inverted brush modes, toggle smoothing, and stroke starting positions (center vs. corner).
    
- Uses `generate_monkey` to create consistent test mesh per backend.
### Out of Scope
- Visual correctness of the brush effect (only numeric validity and change).
    
- Performance, memory, or interaction with automasking.
    
- Brushes not in the Essentials library.
    
- UI or context overrides (handled via `set_view3d_context_override`).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/sculpt_brushes_test.py`
## Approach
The script uses `unittest` with subtests to iterate over backends for each brush. `setUp` is not used; instead `_initialize` resets the scene and generates a monkey mesh for each backend. `_activate_brush` loads the brush from Essentials. `_check_stroke` captures the target attribute (position/mask/face set/color) before and after the stroke, then asserts that all values are finite and at least one changed. The test runs headlessly with Blender’s background mode; `--testdir` must point to the directory containing the test data (though most brushes use generated geometry, not external files).
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/sculpt_brushes_test.py`
