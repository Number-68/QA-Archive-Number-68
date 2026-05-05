# Test Plan: Sculpt Automasking – Face Sets & Topology Isolation Validation
## Description
This test verifies that Blender's sculpt automasking features correctly restrict brush strokes to specific areas of a mesh. It tests two automasking modes: **face set masking** (only vertices belonging to the current face set are affected) and **topology masking** (only vertices in the same connected island as the stroke start are affected). The test uses a prepared monkey mesh with predefined face sets and island IDs, applies a brush stroke at the center of the mesh, and checks that vertex positions within the protected area changed while those outside remained untouched.
## Objective
Ensure that automasking correctly prevents modification of vertices outside the active face set (when `use_automasking_face_sets` is enabled) and outside the active topological island (when `use_automasking_topology` is enabled). The test confirms that at least one vertex inside the protected area is altered, and no vertex outside that area is altered.
## Scope
### In Scope
- Face set automasking – only the active face set (value `3`) should be deformed; other face sets (`1` and `2`) must remain unchanged.
    
- Topology automasking – only the active island (ID `2`) should be deformed; other islands (`0` and `1`) must remain unchanged.
    
- Use of a real brush (Draw brush from Essentials) applied via `sculpt.brush_stroke`.
    
- Vertex position comparisons using numpy before and after the stroke.
    
- Test data: `monkey_realized_island_id_and_face_set.blend` (contains face set and island ID attributes).
### Out of Scope
- Other automasking options (cavity, mesh boundary, etc.).
    
- Performance or memory usage during automasking.
    
- Different brush types or stroke shapes (only a single centered stroke is used).
    
- Multiresolution or dynamic topology sculpting.
    
- UI or user interaction aspects of automasking settings.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/automask_test.py`
## Approach
Automated execution using the provided Python script, which relies on the `unittest` framework. The test loads a prepared `.blend` file containing a monkey mesh with pre-assigned face sets and island IDs. It activates sculpt mode, sets the required automasking option, captures initial vertex positions via `get_attribute_data`, performs a brush stroke at the center of the view using `generate_stroke` and `sculpt.brush_stroke`, then captures final vertex positions. Helper functions filter vertices belonging to or outside the protected region (face set 3 or island 2). The test asserts that vertices inside the protected region changed (at least one), and vertices outside remained identical to their initial positions. The script is intended to be run headlessly via Blender’s background mode with the `--testdir` argument pointing to the test data directory.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/automask_test.py`
