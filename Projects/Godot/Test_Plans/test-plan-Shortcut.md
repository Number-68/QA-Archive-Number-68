# Test Plan: Shortcut – Event Management, Matching, and Text Representation



## Description
This test suite validates Godot's `Shortcut` class, which represents a configurable keyboard shortcut (a set of input events). It verifies that an empty shortcut has no valid events and displays `"None"` as text; that setting and retrieving events preserves the original keycodes; that `set_events_list` works with `List<Ref<InputEvent>>`; that `matches_event` correctly identifies when an event matches any shortcut event (by exact equality, not just same keycode); that `get_as_text()` returns the text representation of the first valid event; that `has_valid_event()` correctly detects the presence of at least one non‑null event; and that `is_event_array_equal` properly compares arrays of events.
## Objective
Ensure that `Shortcut` correctly stores and retrieves a list of input events (keyboard, etc.), that it identifies valid events (non‑null) and reports whether a given event matches the shortcut, that its textual representation matches the underlying event, and that event array comparison works as expected for equality, inequality, and differing lengths.
## Scope
### In Scope
- Default empty shortcut: no valid events, `get_as_text()` returns `"None"`.
    
- Setting events via `set_events(Array)` and retrieving via `get_events()` preserves event keycodes.
    
- `set_events_list(List<Ref<InputEvent>>*)` works similarly.
    
- `matches_event()` returns `true` only for an event that is exactly equal to one in the shortcut (reference equality of the event object, not just same keycode).
    
- `get_as_text()` returns the same string as the first valid event’s `as_text()`.
    
- `has_valid_event()` returns `true` if at least one event in the array is non‑null, `false` otherwise.
    
- `is_event_array_equal()` correctly compares two arrays of events for identical content (order matters).
### Out of Scope
- Shortcut contexts or conflict resolution.
    
- UI integration or editor binding.
    
- Performance or memory.
    
- Other input event types besides key events.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/input/test_shortcut.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` creates `Shortcut` instances and `Ref<InputEventKey>` events, sets properties, and asserts results with `CHECK`. Array comparisons use `is_event_array_equal`. Event equality is based on reference, not keycode alone. The test runs automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/input/test_shortcut.cpp`
