# Test Plan: Sculpt Brush Strength Curves – Stroke Validity Validation
## Description
This test verifies that the Draw brush in Blender's sculpt mode produces valid mesh geometry (no NaN or infinite vertex coordinates) when used with each of the available brush curve distance falloff presets (Smooth, Smoother, Sphere, Root, Sharp, Linear, Sharper, Inverse Square, Constant). For each preset, the test adds a monkey mesh, enters sculpt mode, creates a brush stroke at the view center, and checks that all vertex position components are finite floating‑point numbers.
## Objective
Ensure that none of the brush curve presets cause the sculpting stroke to generate invalid vertex positions (e.g., due to mathematical errors, division by zero, or extreme falloff calculations). The test acts as a regression guard against geometry corruption that could otherwise produce invisible or broken meshes.
## Scope
### In Scope
- All nine brush distance falloff presets: `SMOOTH`, `SMOOTHER`, `SPHERE`, `ROOT`, `SHARP`, `LIN` (linear), `POW4` (sharper), `INVSQUARE`, `CONSTANT`.
    
- A single stroke generated at the view center using `generate_stroke` and applied via `sculpt.brush_stroke`.
    
- The Draw brush (default after entering sculpt mode).
    
- Validation that each vertex position component is neither NaN nor infinite.
### Out of Scope
- Other detail types (`RELATIVE`, `BRUSH`, `MANUAL`).
    
- Different target resolutions or minimum factors.
    
- Performance or memory usage.
    
- Behavior on non‑manifold or complex meshes.
    
- UI or user interaction aspects of dynamic topology.
    
- Other dyntopo operators (e.g., `sculpt.sample_detail_size`).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/sculpt_paint/dyntopo_test.py`
## Approach
Automated execution using the provided Python script, which relies on the `unittest` framework within Blender. The test loads a fresh factory scene, adds a cube, switches to sculpt mode, and enables dynamic topology. It sets the detail method to `CONSTANT` and the constant detail resolution to `1.0`. The `sculpt.detail_flood_fill` operator is called, and the mesh data is refreshed by toggling dynamic topology off and on. Then, the script iterates over all mesh edges, calculates the distance between their vertices using `(v0.co - v1.co).length`, and asserts that each edge length is between `0.4` and `1.0`. The test passes only if all edges satisfy the condition. The script is intended to be run headlessly via Blender’s background mode with the `--testdir` argument (though the test does not load external files; it creates the cube from scratch).
## Test Artifact
- **Automated Script:** `blender/tests/python/sculpt_paint/dyntopo_test.py`
