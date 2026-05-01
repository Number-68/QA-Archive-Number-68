# Test Plan: Armature Modifier – Deformation Modes & Vertex Group Masking Validation
## Description
This test verifies the behavior of Blender's Armature Modifier under various deformation settings, including vertex group binding, bone envelopes, preserve volume, masking, inverse masking, and multiple modifiers on a single mesh. The test operates on both a monkey mesh and a lattice, comparing the final deformed geometry against pre‑stored expected `.blend` files using the custom `mesh_test` framework.
## Objective
Ensure that the Armature Modifier correctly deforms meshes and lattices when different combinations of options are enabled: vertex group weighting (`use_vertex_groups`), bone envelope influence (`use_bone_envelopes`), preserve volume (`use_deform_preserve_volume`), vertex group masking with optional inversion, and stacking multiple armature modifiers in sequence. Each test case confirms that the output mesh matches an approved reference.
## Scope
### In Scope
- Single armature modifier with vertex groups only.
    
- Single armature modifier with bone envelopes only.
    
- Single armature modifier with both vertex groups and bone envelopes.
    
- Preserve volume option during deformation.
    
- Vertex group masking (only a specified group deforms) and its inverse (all but that group deforms).
    
- Two consecutive armature modifiers (multi‑modifier stacking) with and without masking.
    
- Deformation of a lattice object (instead of a mesh).
    
- Comparison of final geometry (vertex positions, topology) against reference `.blend` files.
### Out of Scope
- Performance or memory usage.
    
- Animation or runtime deformations (only static binding and evaluation).
    
- Other modifier types (e.g., Subdivision Surface, Solidify).
    
- UI or user interaction aspects of the Armature Modifier.
    
- Armature constraints or inverse kinematics (only direct modifier deformation).
## Environment
- **OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/modeling/modifiers/armature_modifier.py`
## Approach
Automated execution using the provided Python script, which relies on the `modules.mesh_test` framework. The script builds a list of `SpecMeshTest` entries, each defining: a test name, an input mesh name (e.g., `testMonkeyArmatureDefault`), an expected output mesh name, and a list of `ModifierSpec` (or `MultiModifierSpec` for stacked modifiers). The `RunTest` class loads each input `.blend` file from `tests/files/modeling/modifiers/armature_modifier/`, applies the specified armature modifier(s), evaluates the deformation, and compares the final mesh (vertex‑by‑vertex and topology) against the expected `.blend` file. Results are reported as pass/fail without manual intervention.
## Test Artifact
- **Automated Script:** `blender/tests/python/modeling/modifiers/armature_modifier.py`
