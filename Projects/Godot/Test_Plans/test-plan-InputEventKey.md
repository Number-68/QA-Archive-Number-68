# Test Plan: InputEventKey – State, Modifiers, Matching, and Text Conversion

## Description
This test suite validates Godot's `InputEventKey` class, which represents keyboard key events. It verifies that key press state, keycode (logical) and physical keycode, modifier flags (Ctrl, Alt, Shift), unicode character, echo flag, and key location are correctly stored and retrieved. It also checks key‑to‑text conversion (`as_text()`), string representation (`to_string()`), reference creation, and both action‑based matching and exact `is_match()` comparison.
## Objective
Ensure that `InputEventKey` correctly manages all keyboard‑related properties: pressed/unpressed state, logical keycode and physical keycode (with fallback when logical is `Key::NONE`), modifier mask combination, unicode character, echo state, and key location (left/right/unspecified). Verify that `as_text()` produces human‑readable strings with modifiers ordered consistently (macOS vs. other platforms), `to_string()` includes all relevant fields, `action_match()` correctly compares against an `InputEventKey` reference (including deadzone‑adjusted strength), and `is_match()` supports exact modifier/location matching.
## Scope
### In Scope
- Pressed state getter/setter and echo flag.
    
- Keycode (logical) and physical keycode storage and retrieval.
    
- Combined keycode with modifiers (`get_keycode_with_modifiers`, `get_physical_keycode_with_modifiers`).
    
- Unicode character storage.
    
- Key location (unspecified, left, right).
    
- `as_text()` output for various combinations (none, modifiers alone, keycode only, physical fallback, modifier ordering).
    
- `to_string()` output for different states (modifiers, location, pressed, echo).
    
- `create_reference(Key)` to generate a `Ref<InputEventKey>` with given keycode.
    
- Action matching (`action_match`) against another event, including strength and pressed state detection.
    
- Exact match (`is_match`) with and without modifier/location checking.
### Out of Scope
- Real‑time input handling or device event generation.
    
- Performance or memory usage.
    
- UI or editor integration.
    
- Other input event types (mouse, joystick).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/input/test_input_event_key.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` instantiates `InputEventKey`, sets properties via setter methods, and asserts values with `CHECK` and `CHECK_FALSE`. `as_text()` and `to_string()` outputs are compared against hardcoded strings; platform differences (macOS vs. others) are handled with `#ifdef MACOS_ENABLED`. Action matching tests verify that both keycode and physical‑keycode fallback work, and that modifier mismatches cause false results. `is_match()` is tested with exact modifier/location matching. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/input/test_input_event_key.cpp`
