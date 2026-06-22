# Test Plan: AABB – Axis‑Aligned Bounding Box Math Operations

## Description
This test suite validates Godot's `AABB` (Axis‑Aligned Bounding Box) class, which represents a 3D rectangular box aligned with the coordinate axes. It tests constructors, string conversion, basic getters/setters, volume and surface area calculations, intersection tests (with other AABBs, planes, segments, rays), merging, enclosing, endpoint enumeration, longest/shortest axis queries, support point calculation, growing (expanding/shrinking), point containment, expansion to include points, and finite‑number validation.
## Objective
Ensure that `AABB` correctly computes its position, size, end, center, volume, and surface; accurately determines intersections with other AABBs, planes, segments, and rays (including edge cases like zero‑length segments, zero‑size AABBs, and rays starting inside/outside); correctly merges two AABBs; correctly detects encloses relationships; returns the correct endpoints, axes, and support points; expands and grows correctly; accurately tests point containment (including boundary edges); and correctly identifies whether all components are finite.
## Scope
### In Scope
- Constructors: default, from position+size, copy.
    
- String conversion to `[P: (...), S: (...)]`.
    
- Getters: `get_position`, `get_size`, `get_end`, `get_center`.
    
- Setters: `set_end`, `set_position`, `set_size` (including negative sizes).
    
- Volume and surface: `get_volume` (handles negative size components), `has_volume`, `has_surface`.
    
- Intersection: `intersects` with AABB, `intersection` (returns overlapping AABB), `intersects_plane`, `intersects_segment`, `intersects_ray` (including zero‑length and parallel rays).
    
- Ray intersection details: `find_intersects_ray` returns `inside` flag, intersection point, and normal (handles borders and inside cases).
    
- Merging: `merge` with contained, partially contained, and non‑contained AABBs.
    
- Encloses: check if one AABB fully contains another (including boundary touches).
    
- Endpoints: `get_endpoint` for all 8 corners (invalid indices return zero).
    
- Longest/shortest axis: axis vector, index, and size.
    
- Support point: `get_support` returns farthest corner along a direction.
    
- Grow: expand/shrink by a uniform amount (handles negative values).
    
- Point containment: `has_point` (includes boundary).
    
- Expand: enlarge to include a point.
    
- Finite check: `is_finite` detects NaN/infinite components.
### Out of Scope
- AABB transformations (rotation, scaling).
    
- Performance or memory.
    
- Other collision shapes (sphere, capsule).
    
- UI or rendering.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/math/test_aabb.cpp
## Approach
The test uses Godot's built‑in doctest framework. Each `TEST_CASE` creates `AABB` instances with known values, calls methods, and compares results using `CHECK_MESSAGE` and `is_equal_approx` for floating‑point tolerance. Edge cases (negative sizes, zero‑sized AABBs, invalid indices, infinite components) are explicitly tested. Ray intersection tests cover multiple scenarios: rays from outside, inside, parallel, zero‑length, and diagonal. The test runs automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/math/test_aabb.cpp`
