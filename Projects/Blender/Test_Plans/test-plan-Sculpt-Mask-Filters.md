# Test Plan: Sculpt Mask Filters – Grow, Shrink, Clear, Invert, By Color & Cavity Validation

## Description
This test validates Blender’s sculpt mask operators on a sphere with partial masking. It verifies that `mask_filter` (GROW/SHRINK) changes vertex mask counts correctly, `mask_flood_fill` (VALUE/INVERT) removes or inverts masks, `mask_by_color` masks red vertices on a plane, and `mask_from_cavity` masks elevated areas of a wavy plane.
## Objective
Ensure that all mask operators produce the expected mask values and attribute states: GROW increases masked vertices, SHRINK decreases them, VALUE=0 removes the mask attribute, INVERT flips mask values (1‑x) and creates full mask on unmasked meshes, mask_by_color masks only similarly colored vertices, and mask_from_cavity masks vertices above a height threshold.
## Scope
### In Scope
- GROW and SHRINK filters – compare non‑zero mask counts before/after.
    
- VALUE=0 flood fill – verify `.sculpt_mask` attribute disappears.
    
- INVERT on partial mask – each vertex becomes `1.0 - old_value`.
    
- INVERT on unmasked mesh – all vertices become `1.0`.
    
- mask_by_color – off‑grid returns CANCELLED and no mask; on red circle masks red vertices only.
    
- mask_from_cavity on a wavy plane – vertices with Z>0 are masked, Z<0 are not.
    
- Test files: `partially_masked_sphere.blend`, `plane_with_red_circle.blend`, `plane_with_valley.blend`.
### Out of Scope
- Performance, memory, UI, or brush strokes.
    
- Other mask filters (smooth, contrast, etc.).
    
- Masking on non‑mesh objects.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/mask_test.py`
## Approach
The script uses `unittest` within Blender. Each test loads a specific `.blend` file from `--testdir`, reads mask or color attributes via `numpy`, runs the operator, then reads the updated attributes for comparison with expected values. Counts of masked vertices are compared (GROW/SHRINK), attribute existence (`VALUE=0`) is checked, and per‑vertex values are compared with tolerance (INVERT, mask_by_color, mask_from_cavity). The script runs headlessly with Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/mask_test.py`
