# Test Plan: InputEvent – Signal Emission, Accumulation, Action Mapping, and Transformation
## Description
This test suite validates core behavior of Godot's `InputEvent` classes. It checks that changing an event's device ID emits the `changed` signal, that mouse motion events accumulate correctly, that input events can be matched to named actions (via `InputMap`) with proper pressed/released states and strengths, and that events can be transformed by a `Transform2D`.
## Objective
Ensure that `InputEvent` objects behave as expected: the `changed` signal fires when device ID changes; `accumulate()` returns true only for compatible events of the same type with matching metadata; events correctly query `InputMap` actions, including deadzone‑adjusted strength and pressed/released status; and `xformed_by()` applies a 2D transformation to position and global position.
## Scope
### In Scope
- Signal emission on device change (`InputEventKey`).
    
- `accumulate` for `InputEventMouseMotion` (same button mask).
    
- Incompatible event types do not accumulate.
    
- Action mapping: adding an action to `InputMap`, associating a joystick motion event, and testing `is_action_type()`, `is_action()`, `is_action_pressed()`, `is_action_released()`, `get_action_strength()`, `get_action_raw_strength()`.
    
- Transforming an event by a `Transform2D` (position updated).
### Out of Scope
- Other input event types (touch, pen, etc.).
    
- Real‑time input processing or device polling.
    
- Performance or memory usage.
    
- UI or editor integration.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/input/test_input_event.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` creates event instances, sets properties, and asserts results with `CHECK` and `CHECK_FALSE`. Signal tests use `SIGNAL_WATCH`/`SIGNAL_CHECK` macros. Action mapping tests temporarily register a mock action in `InputMap` and clean it up. Transformation tests compare approximated vector positions. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/input/test_input_event.cpp`
