# Test Plan: Sculpt Mesh Filters – Numerical Validity & Change Detection

## Description
This test validates that each sculpt mesh filter (SMOOTH, SURFACE_SMOOTH, INFLATE, RELAX, RELAX_FACE_SETS, SHARPEN, ENHANCE_DETAILS, SCALE, SPHERE, RANDOM) produces valid vertex positions (no NaN or infinite values) and actually modifies the mesh geometry. It runs each filter on a prepared monkey mesh (`30k_monkey_mask_and_face_set.blend`) and compares vertex coordinates before and after.
## Objective
Ensure that every mesh filter generates rational position coordinates and that at least one vertex changes position. This catches filters that crash, produce broken geometry, or silently do nothing. It also guards against accidental introduction of invalid floating‑point values.
## Scope
### In Scope
- Filters: SMOOTH, SURFACE_SMOOTH, INFLATE, RELAX, RELAX_FACE_SETS, SHARPEN (plain and with options: intensify detail strength, curvature smooth iterations), ENHANCE_DETAILS, SCALE, SPHERE, RANDOM.
    
- Test asset: `30k_monkey_mask_and_face_set.blend` (a monkey with mask and face set data).
    
- Validity check: every vertex coordinate component (X,Y,Z) must be finite (not NaN, not infinite).
    
- Change check: at least one vertex coordinate differs from its original value.
### Out of Scope
- Visual correctness of the filter effect (only numeric validity and change).
    
- Performance or memory usage.
    
- Filters other than those listed.
    
- UI, context overrides, or brush strokes.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/mesh_filter_test.py`
## Approach
The script uses `unittest` within Blender. After loading the test blend and entering sculpt mode, each test method calls a helper `_check_filter(type, opts)`. The helper captures initial vertex positions via `get_attribute_data`, applies the mesh filter using `sculpt.mesh_filter`, then captures new positions. It asserts that all new coordinate components are finite (`math.isinf`/`math.isnan`) and that at least one component has changed. The script runs headlessly with Blender’s background mode and requires `--testdir` pointing to the directory containing the test blend.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/mesh_filter_test.py`
