# Test Plan: Sculpt Brush Strength Curves – Stroke Validity Validation
## Description
This test verifies that the Draw brush in Blender's sculpt mode produces valid mesh geometry (no NaN or infinite vertex coordinates) when used with each of the available brush curve distance falloff presets (Smooth, Smoother, Sphere, Root, Sharp, Linear, Sharper, Inverse Square, Constant). For each preset, the test adds a monkey mesh, enters sculpt mode, creates a brush stroke at the view center, and checks that all vertex position components are finite floating‑point numbers.
## Objective
Ensure that none of the brush curve presets cause the sculpting stroke to generate invalid vertex positions (e.g., due to mathematical errors, division by zero, or extreme falloff calculations). The test acts as a regression guard against geometry corruption that could otherwise produce invisible or broken meshes.
## Scope
### In Scope
- - All nine brush distance falloff presets: `SMOOTH`, `SMOOTHER`, `SPHERE`, `ROOT`, `SHARP`, `LIN` (linear), `POW4` (sharper), `INVSQUARE`, `CONSTANT`.
    
- A single stroke generated at the view center using `generate_stroke` and applied via `sculpt.brush_stroke`.
    
- The Draw brush (default after entering sculpt mode).
    
- Validation that each vertex position component is neither NaN nor infinite.
### Out of Scope
- - Other brush types (Smooth, Mask, Crease, etc.).
    
- Different stroke shapes or multiple strokes.
    
- Performance or memory usage.
    
- Visual appearance of the stroke or falloff correctness – only numeric validity is checked.
    
- Other automasking or sculpt settings.
    
- Interaction with brush textures or other brush parameters.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/brush_strength_curves_test.py`
## Approach
Automated execution using the provided Python script, which relies on the `unittest` framework within Blender. The test creates a fresh factory scene, adds a monkey mesh, and enters sculpt mode. Each test method sets a different `curve_distance_falloff_preset` on the active brush, then calls a helper function that constructs a view context override, generates a single stroke at the center of the view, and retrieves all vertex positions from the mesh's `position` attribute. Using `numpy`, it checks that every coordinate component is finite (not NaN or infinite) with `math.isinf` and `math.isnan`. The script is intended to be run headlessly via Blender’s background mode with the `--testdir` argument pointing to the test data directory (though the test does not actually load any external `.blend` file; it uses the factory‑generated monkey).
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/brush_strength_curves_test.py`
