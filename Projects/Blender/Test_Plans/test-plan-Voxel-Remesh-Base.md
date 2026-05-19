# Test Plan: Voxel Remesh – Basic Cube Remeshing & Zero Size Handling
## Description
This test validates the `object.voxel_remesh` operator on a simple cube. It checks that with default settings (voxel size 0.1) the resulting mesh has exactly 2,648 vertices. It also verifies that setting voxel size to 0 causes a `RuntimeError`, preventing an invalid remesh.
## Objective
Ensure that voxel remeshing produces a predictable, fixed vertex count on a cube, and that the operator fails gracefully (raises error) when voxel size is zero – avoiding hangs or corruption.
## Scope
### In Scope
- Cube primitive, sculpt mode enabled.
    
- Voxel size 0.1 → vertex count must be 2648.
    
- Voxel size 0 → operator must raise `RuntimeError`.
    
- Operator: `object.voxel_remesh`.
    
- Runs headlessly with factory settings.
### Out of Scope
- Other meshes (monkey, sphere).
    
- Adaptivity, fix poles, or preserve volume options.
    
- Performance or memory.
    
- Color attributes or UVs.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/voxel_remesh_test.py`
## Approach
Uses `unittest` inside Blender. `setUp` creates a cube and enters sculpt mode. First test sets `remesh_voxel_size = 0.1`, runs the operator, and asserts the vertex count equals 2648. Second test sets size to 0, wraps the operator in `assertRaises(RuntimeError)`. The test script runs headlessly with Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/voxel_remesh_test.py`
