# Test Plan: AStar3D – Pathfinding Algorithm, Graph Operations, and Disabled Points

## Description
This test suite validates Godot's `AStar3D` class, which implements the A* pathfinding algorithm for 3D graphs. It tests pathfinding on a custom graph with overridden costs (ABCX), basic add/remove/connect/disconnect operations, directed vs. undirected edge handling, closest position in segment queries, random graph connectivity and point removal stress tests, and disabled points returning empty paths.
## Objective
Ensure that `AStar3D` correctly finds the shortest path in a weighted graph, respects directed/undirected edges, maintains graph integrity after adding and removing points and edges, correctly computes the closest position on a segment for any point, handles disabled points (returns empty path), and remains stable under random edge manipulation and point removal stress.
## Scope
### In Scope
- Pathfinding on a custom graph (ABCX) with overridden cost function – verifies expected path order (X → A → B → C).
    
- Manual add/remove of points and edges: `add_point`, `remove_point`, `connect_points`, `disconnect_points`, `are_points_connected`.
    
- Directed vs. undirected connections: symmetric vs. asymmetric edge behavior.
    
- `get_point_connections` returns correct neighbor lists.
    
- `get_closest_position_in_segment` returns nearest point on any graph edge.
    
- Random edge addition/removal stress (20,000 iterations) with connectivity checks.
    
- Random point removal stress (20,000 iterations) – ensures no leftover edges to removed points.
    
- Disabled points (`set_point_disabled(true)`) return empty path and point path vectors.
### Out of Scope
- Alternative heuristics or custom cost functions beyond the overridden `_compute_cost`.
    
- Performance benchmarking.
    
- Thread safety.
    
- Other A* variants (AStar2D, AStarGrid2D).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/math/test_astar.cpp
## Approach
The test uses Godot's built‑in doctest framework. A custom `ABCX` class overrides `_compute_cost` to create a specific pathfinding scenario. Each `TEST_CASE` performs operations and asserts results with `CHECK` and `REQUIRE`. Random stress tests use `Math::rand()` with `Math::seed(0)` for reproducibility. Disabled point tests verify that both `get_id_path` and `get_point_path` return empty vectors. The test runs automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/math/test_astar.cpp`
