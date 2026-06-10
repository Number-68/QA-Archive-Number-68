# Test Plan: Resource – Duplication, Saving, Loading, and Circular Reference Handling
## Description
This test suite validates Godot’s `Resource` class for duplication (shallow, deep, deep with mode, for local scene), property usage flags (`PROPERTY_USAGE_ALWAYS_DUPLICATE`, `NEVER_DUPLICATE`, `STORAGE`), and the interplay with `Variant::duplicate`/`duplicate_deep`. It also tests saving to binary and text formats with metadata (including nested resources) and loading back, and verifies that circular references are broken on save (meta references are cleared) to prevent infinite loops.
## Objective
Ensure that `Resource::duplicate` (shallow/deep) and `duplicate_deep` with different modes correctly copy properties and sub‑resources according to their property usage flags and built‑in/external status. Verify that `Variant::duplicate` and `duplicate_deep` behave similarly when the root is a resource. Confirm that `duplicate_for_local_scene` handles scene‑local subresources properly. Validate that saving and loading resources preserves names, metadata, and nested resources, and that circular references in metadata are broken (set to null) during save to avoid serialization cycles.
## Scope
### In Scope
- Duplication modes: shallow, deep, `duplicate_deep` with `RESOURCE_DEEP_DUPLICATE_NONE/INTERNAL/ALL`.
    
- Property usage flags (`PROPERTY_USAGE_NONE`, `ALWAYS_DUPLICATE`, `STORAGE`, `NEVER_DUPLICATE`) affecting duplication.
    
- Built‑in vs. external resources (by path) – internal ones duplicated, external ones referenced unless `ALL` mode.
    
- Scene‑local subresources (`set_local_to_scene`) – duplication for local scene replaces copies.
    
- Variant duplication of a resource (shallow, deep, deep with mode) – sharing detection across nested structures.
    
- Saving `Resource` with metadata (string, vector, child resource) to binary (`.res`) and text (`.tres`) formats.
    
- Loading back and verifying name, metadata values, child resource name.
    
- Circular reference in metadata (`A -> B -> C -> B`): saved, loaded, and the circular entry becomes null.
### Out of Scope
- Other `Resource` types (e.g., `Script`, `Texture`).
    
- Threading or performance.
    
- File system permissions or path errors.
    
- Encryption or compression.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_resource.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. It defines a helper class `DuplicateGuineaPigData` that holds a complex set of properties (arrays, dictionaries, sub‑resources, scene‑local resources) and a set of test resource classes with different property usage flags. A lambda `_run_test` executes duplication for each test mode and compares original and duplicate using `verify_duplication` (which checks duplication correctness via a hashmap of resource pointers). Saving/loading tests write to temporary files, reload with `ResourceLoader`, and assert metadata values. Circular references are built using meta links, saved, loaded, and checked that the cycle is broken. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_resource.cpp`
