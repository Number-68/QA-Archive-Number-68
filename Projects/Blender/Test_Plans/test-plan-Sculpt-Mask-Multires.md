# Test Plan: Multires Modifier – Apply Base After Subdivision Validation

## Description
This test checks that applying the base level of a Multires modifier after a single subdivision produces a mesh identical to a known‑good reference. It loads a prepared `.blend` file containing an expected result mesh, creates a new monkey primitive, subdivides it once with Multires, applies the base, and compares the final geometry.
## Objective
Ensure that `object.multires_base_apply` correctly makes the subdivided mesh the new base level, and that the resulting mesh matches the expected shape (no distortion or data loss).
## Scope
### In Scope
- Single subdivision via `object.multires_subdivide`.
    
- Applying the base level via `object.multires_base_apply`.
    
- Mesh comparison using `unit_test_compare` (returns `'Same'`).
    
- Test file: `apply_base_monkey.blend` (contains `Expected_Base_Mesh`).
### Out of Scope
- Multiple subdivision levels.
    
- Reshaping or sculpting between subdivisions.
    
- Performance or memory usage.
    
- Other modifiers or mesh types.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/multires_operators_test.py`
## Approach
The script uses `unittest` inside Blender. `setUp` loads the test blend file. The single test method adds a new monkey, adds a Multires modifier, subdivides once, applies the base, removes the modifier, and compares the resulting mesh against the pre‑stored `Expected_Base_Mesh` using the mesh’s `unit_test_compare` method. The test passes if the returned string is `'Same'`. It runs headlessly with Blender’s background mode; `--testdir` must point to the directory containing the test `.blend`.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/multires_operators_test.py`
