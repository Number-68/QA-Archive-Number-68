# Test Plan: Modifiers Legacy Test Suite – Comprehensive Modifier Validation
## Description
This test file contains a broad collection of tests that validate many of Blender's mesh modifiers, including Generate modifiers (Array, Bevel, Boolean, Decimate, Mirror, Screw, Solidify, Subdivision Surface, Triangulate, Wireframe, etc.) and Deform modifiers (Armature, Cast, Curve, Displace, Lattice, Shrinkwrap, Simple Deform, Smooth, Wave, etc.). It also includes tests for curves with modifiers. Each test applies one or more modifiers to a base mesh or curve and compares the final geometry against a pre‑stored expected `.blend` file. The tests are executed using the custom `mesh_test` framework.
## Objective
Ensure that a wide range of modifiers produce correct geometry under various settings, that modifier stacks (multiple modifiers in sequence) evaluate correctly, and that historical regression bugs (e.g., T58411, T72380, T61979) remain fixed. The test suite acts as a safety net for changes to the modifiers system, preventing regressions across many modifier types.
## Scope
### In Scope
- All modifier types explicitly listed in the test list: Array, Bevel, Boolean, Build, Cast, Curve, Decimate (Collapse, Planar, Unsubdivide), Edge Split, Hook (commented but present), Laplacian Smooth, Lattice, Mask, Mirror, Screw, Shrinkwrap (Target Normal Project), Simple Deform, Skin, Smooth, Solidify, Subdivision Surface (Catmull‑Clark, Simple, with creases), Triangulate, Wave, Weld.
    
- Tests on mesh primitives (cube, sphere, cylinder, cone, torus, plane) and on curves (Bezier).
    
- Chained modifier stacks (e.g., Mirror → Bevel → Subsurf).
    
- Specific regression tests (T58411, T72380, T61979, T63063, T72792).
    
- Use of vertex groups, object offsets, helper objects (curves, lattices, empties, arrays), and vertex weight editing.
    
- Deterministic random seed (`seed(0)`) for reproducibility.
### Out of Scope
- Modifiers not listed (e.g., Fluid, Cloth, Soft Body, Particle System, Ocean).
    
- Performance or memory usage under heavy modifier stacks.
    
- Animation or runtime deformation over time (most tests are static, except Build which evaluates at a specific frame).
    
- UI or user interaction aspects of modifiers.
    
- New modifiers added after this file was deprecated (the header advises adding new tests to separate files, not here).
    
- Testing of the `mesh_test` framework itself.
## Environment
- **OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/modeling/modifiers/modifiers.py`
## Approach
Automated execution using the provided Python script (`modifiers_test.py`), which relies on the `modules.mesh_test` framework. The script defines a list of `SpecMeshTest` entries, each specifying an input mesh name, an expected output mesh name, and a sequence of `ModifierSpec` (or `MultiModifierSpec` for stacked modifiers). The `RunTest` class loads each input `.blend` file from `tests/files/modeling/modifiers/`, applies the specified modifiers in order, forces dependency graph evaluation, and compares the resulting mesh geometry (vertex positions and topology) against the expected `.blend` file. A test passes if the meshes match within tolerance; otherwise it fails. The script is intended to be run headlessly via Blender’s background mode, as shown in the docstring comment.
## Test Artifact
- **Automated Script:** `blender/tests/python/modeling/modifiers/modifiers.py`
