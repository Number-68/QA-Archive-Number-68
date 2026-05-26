# Test Plan: InputEventMouse – Button Mask, Position, and Global Position

## Description
This test suite validates Godot's `InputEventMouse` class, which represents mouse input events. It verifies that the mouse button mask (left, right, middle, XButton1, XButton2) can be set and retrieved correctly, and that both the local position and global position of the mouse are properly stored and retrieved, including negative values.
## Objective
Ensure that `InputEventMouse` correctly manages button mask flags (each bit‑flag can be set independently and checked via `has_flag`), and that `set_position`/`get_position` and `set_global_position`/`get_global_position` work as expected for both positive and negative coordinate values.
## Scope
### In Scope
- Setting and retrieving individual mouse button masks: `LEFT`, `RIGHT`, `MIDDLE`, `MB_XBUTTON1`, `MB_XBUTTON2`.
    
- Verifying button mask flags with `has_flag()`.
    
- Setting local position and retrieving it.
    
- Setting global position and retrieving it.
    
- Positive and negative coordinate values.
### Out of Scope
- Mouse motion, wheel events, or double‑click.
    
- Input mapping or action matching.
    
- Performance or memory usage.
    
- UI or editor integration.
    
- Other input event types (key, joypad).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/input/test_input_event_mouse.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` instantiates `InputEventMouse`, calls setter methods, and asserts the getter results with `CHECK`. Button mask tests use `has_flag()` to confirm individual bits. Position tests compare `Vector2` values directly. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/input/test_input_event_mouse.cpp`
