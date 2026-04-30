# Test Plan: Multiresolution Modifier – Subdivision Modes Validation.
## Description
This test verifies the behavior of Blender's Multiresolution Modifier when applying its three subdivision modes (Catmull‑Clark, Simple, and Linear) to different base meshes. It uses a custom mesh testing framework (`modules.mesh_test`) that applies a sequence of modifiers and operators, then compares the resulting mesh geometry against pre‑stored expected `.blend` files. The test covers a cube (for basic validation) and the Suzanne monkey (for more complex geometry) to ensure subdivisions produce correct vertex counts, positions, and topology.
## Objective
Ensure that `object.multires_subdivide` with each mode (`'CATMULL_CLARK'`, `'SIMPLE'`, `'LINEAR'`) produces mesh output identical to the known‑good reference meshes stored in the test data directory. The test also confirms that applying the Multiresolution Modifier after subdivision yields final geometry that matches expectations across different mesh complexities.
## Scope
### In Scope
- Catmull‑Clark subdivision on a cube and Suzanne.
    
- Simple subdivision (no smoothing) on a cube and Suzanne.
    
- Linear subdivision (preserve original shape) on a cube and Suzanne.
    
- Applying the Multiresolution Modifier after subdivision is complete.
    
- Comparison of resulting mesh against expected `.blend` files (vertex positions, face counts, overall shape).
### Out of Scope
- Performance or memory usage during subdivision.
    
- Other modifiers or operator combinations outside the Multiresolution Modifier.
    
- Subdivision at levels beyond the first subdivision (the test only subdivides once).
    
- Testing with animated meshes or deformations.
    
- UI aspects of the Multiresolution Modifier.
## Environment
- **OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/modeling/modifiers/multires_modifier.py`
## Approach
Automated execution using the provided Python script, which relies on a custom mesh testing framework (`modules.mesh_test`). The script defines a list of `SpecMeshTest` entries, each specifying an input mesh name, a sequence of `ModifierSpec` and `OperatorSpec` steps, and an expected output mesh name. The `RunTest` class executes each specification: loads the input `.blend` file, applies the modifier and operators, compares the resulting mesh with the expected reference (via vertex‑by‑vertex or mesh‑difference comparison), and reports pass/fail. The test runs headlessly using Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/modeling/modifiers/multires_modifier.py`
