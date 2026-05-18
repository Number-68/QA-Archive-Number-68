# Test Plan: Voxel Remesh – Color Interpolation & Mesh Comparison

## Description
This test validates that Blender’s voxel remesh operator preserves or correctly interpolates vertex colors when converting a mesh to a voxel‑based topology. It loads a test cube with color data, applies the voxel remesh, and compares the resulting mesh against an expected reference.
## Objective
Ensure that `object.voxel_remesh` produces a mesh that matches a known‑good reference, specifically verifying that color attributes are handled correctly during remeshing.
## Scope
### In Scope
- Single test: `SpecMeshTest("Color Interpolation", "testCube", "expectedCube", [OperatorSpec(...)])`.
    
- Operators: `ed.undo_push` and `object.voxel_remesh` (default settings).
    
- Mesh comparison via `mesh_test` framework (likely vertex‑by‑vertex with attributes).
    
- Test data loaded from `tests/files/sculpting/voxel_remesh_compare/`.
### Out of Scope
- Different remesh settings (voxel size, adaptivity, etc.).
    
- Performance or memory usage.
    
- Other attributes (UVs, normals) unless part of the reference.
    
- Interactive or UI aspects.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/voxel_remesh_compare_test.py`
## Approach
The script uses the custom `mesh_test` framework. It registers a single `SpecMeshTest` that loads a test cube, pushes an undo step, runs `object.voxel_remesh`, and then compares the resulting mesh with the `expectedCube` reference. The comparison is done automatically by the framework. The test runs headlessly with Blender’s background mode; the command‑line includes a directory path to the test data.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/voxel_remesh_compare_test.py`
