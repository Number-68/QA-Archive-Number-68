# Test Plan: Weight Paint Brushes – Weight Data Validity on Stroke
## Description
This test validates weight paint brushes (Paint, Average, Blur, Smear) on a 30k‑vertex monkey mesh. For each brush, it performs a stroke at view center and checks that all vertex weight values remain finite (no NaN or infinite) and that at least one weight changes.
## Objective
Ensure that weight paint strokes produce valid weight data and actually modify the mesh’s vertex group weights. Catches brush code that could corrupt weight maps or fail silently.
## Scope
### In Scope
- Brushes tested: Paint, Average, Blur, Smear.
    
- Test mesh: `30k_monkey.blend` (pre‑loaded with a vertex group named "Group").
    
- Stroke at default location via `generate_stroke`.
    
- Validation: all weight values finite and at least one weight different after stroke.
### Out of Scope
- Other weight paint brushes (e.g., Weight Gradient, Clean).
    
- Masking, pressure sensitivity, or brush textures.
    
- Performance or memory usage.
    
- UI or viewport rendering.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/weight_paint_brushes_test.py`
## Approach
The script uses `unittest` within Blender. `setUp` loads the test blend and toggles weight paint mode. Each test activates a brush, captures initial weights from vertex group 0, applies a stroke using `paint.weight_paint`, and captures new weights. Assertions check that all weights are finite and at least one weight changed. The test runs headlessly with `--testdir` pointing to the directory containing `30k_monkey.blend`.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/weight_paint_brushes_test.py`
