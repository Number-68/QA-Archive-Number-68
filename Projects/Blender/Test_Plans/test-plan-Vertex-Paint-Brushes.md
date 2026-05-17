# Test Plan: Vertex Paint Brushes – Color Data Validity on Stroke

## Description
This test validates several vertex paint brushes (Airbrush, Paint Hard/Pressure, Paint Soft/Pressure, Average, Blur, Smear) on a 30k‑vertex monkey mesh. For each brush, it performs a stroke at view center and checks that all color channel values (RGBA per corner) remain finite and that at least one channel changes.
## Objective
Ensure that vertex paint strokes produce valid color data (no NaN or infinite values) and actually modify the mesh’s vertex colors. Catches brush code that could corrupt color attributes or fail silently.
## Scope
### In Scope
- Brushes tested: Airbrush, Paint Hard, Paint Hard Pressure, Paint Soft, Paint Soft Pressure, Average, Blur, Smear.
    
- Test mesh: `30k_monkey.blend` (pre‑loaded).
    
- Stroke at default location via `generate_stroke`.
    
- Validation: all color channels finite and at least one channel different after stroke.
### Out of Scope
- Other vertex paint brushes (e.g., Fill, Clone).
    
- Masking, pressure sensitivity, or brush textures.
    
- Performance or memory usage.
    
- UI or viewport rendering.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/vertex_paint_brushes_test.py`
## Approach
The script uses `unittest` within Blender. `setUp` loads the test blend and toggles vertex paint mode. Each test method activates a specific brush, captures initial color attribute data (CORNER domain, RGBA) via `get_attribute_data`, applies a stroke using `paint.vertex_paint`, then captures new data. Assertions check that all channels are finite and at least one channel changed. The test runs headlessly with `--testdir` pointing to the directory containing `30k_monkey.blend`.
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/vertex_paint_brushes_test.py`
