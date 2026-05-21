# Test Plan: ProjectSettings – Core Configuration Management Tests
## Description
This test suite validates Godot's `ProjectSettings` singleton, which manages engine and game configuration. It verifies that settings can be retrieved, set, and defaulted correctly; that missing settings return `nil` or a provided default; that path localization converts absolute paths to `res://` URIs across platforms; and that the `settings_changed` signal is emitted with proper change tracking and clearing after signal dispatch.
## Objective
Ensure `ProjectSettings` API behaves predictably: existing settings return correct types and values; non‑existent settings return `nil` or a default without creating a permanent entry; `localize_path()` transforms file paths to project‑relative `res://` URIs; changed settings are tracked, grouped, and cleared only after `settings_changed` signal is flushed; and setting an identical value does not trigger change tracking.
## Scope
### In Scope
- Retrieving existing and non‑existing settings with/without default values.
    
- Setting a new custom setting and retrieving it.
    
- `localize_path()` for relative paths, `..`/`.` resolution, absolute filesystem paths, and platform‑specific Windows paths.
    
- `settings_changed` signal emission and its interaction with `MessageQueue` flushing.
    
- `get_changed_settings()` and `check_changed_settings_in_group()`.
    
- Clearing changed settings after signal emission.
    
- Avoiding duplicate change tracking when setting same value.
### Out of Scope
- Saving/loading `project.godot` file.
    
- Override layers (feature tags, command line).
    
- Performance or memory usage.
    
- UI or editor integration.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/config/test_project_settings.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` operates on the global `ProjectSettings` singleton. Assertions use `CHECK`, `CHECK_EQ`, and `CHECK_FALSE`. Signal tests use `SIGNAL_WATCH`/`SIGNAL_CHECK` and manually flush `MessageQueue` to process delayed signal delivery. Path tests run conditionally for Windows and POSIX. All tests run automatically via Godot’s `--test` command line.
## Test Artifact
- **Automated Script:** `godot/tests/core/config/test_project_settings.cpp`
