# Test Plan: Texture Paint Brushes – Image Data Validity on Stroke

## Description
This test validates the Paint Hard brush in texture paint mode. It creates a monkey mesh, adds a blank 512x512 texture (either byte or float), performs a single brush stroke at the view center, and checks that all pixel color channels remain finite (no NaN or infinite) and that at least one pixel changed. The test requires experimental texture paint features enabled and runs only in alpha builds.
## Objective
Ensure that a basic texture paint stroke produces valid image data (no floating‑point errors) and actually modifies the texture. This catches brush code that could corrupt images or fail silently.
## Scope
### In Scope
- Paint Hard brush from Essentials library.
    
- Image data types: 8‑bit byte and 32‑bit float.
    
- Stroke at default location (center) via `generate_stroke`.
    
- Validation: all pixel channel values are finite (not NaN/inf) and at least one differs from initial state.
### Out of Scope
- Other brushes (only Paint Hard).
    
- Masking, pressure sensitivity, or brush textures.
    
- Performance or memory usage.
    
- UI or viewport rendering.
    
- Non‑alpha Blender versions (test is skipped if not `'alpha'`).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/texture_paint_brushes_test.py`
## Approach
The script uses `unittest` with subtests for byte/float data types. It enables the experimental texture paint preference, loads a fresh scene, generates a monkey mesh, and adds a blank image texture. It then activates the Paint Hard brush, captures initial image pixels, applies a stroke via `sculpt.brush_stroke`, and captures new pixels. Assertions check that all channels are finite and that at least one channel changed. The test runs headlessly with `--testdir` pointing to the test data directory (though it generates geometry, not external files).
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/texture_paint_brushes_test.py`
