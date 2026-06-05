# Test Plan: JSON Native – Type‑Preserving Conversion Between Variant and JSON

## Description
This test validates Godot’s `JSON::from_native` and `JSON::to_native` functions, which encode arbitrary Variant data (including typed arrays/dictionaries, math types, resources) into a custom JSON format that preserves type information (e.g., `"i:123"` for integer, `"s:abc"` for string, objects with `"type":"Vector3"` and `"args":[...]`). It tests round‑trip serialization and deserialization for all basic types, math types (Vector, Rect, Transform, Basis, Projection, Color, etc.), native types (`RID`, `Callable`, `Signal`, `Resource` with/without full objects flag), arrays, typed arrays, dictionaries, typed dictionaries, and packed arrays.
## Objective
Ensure that native Variant data can be losslessly converted to a JSON string and back, preserving type identity, structure, and values for all supported types. Verify that `full_objects` flag enables serialization of `Object`‑derived types (e.g., `Resource`) and that disabling it replaces them with `null`. Confirm that non‑serializable types (`RID`, `Callable`, `Signal`) produce placeholder objects and become empty on decode unless full objects are allowed.
## Scope
### In Scope
- Basic types: `nil`, `bool`, integer, float (including `inf`, `-inf`, `nan`), `String`, `StringName`, `NodePath`.
    
- Math types: `Vector2`, `Vector2i`, `Rect2`, `Rect2i`, `Vector3`, `Vector3i`, `Transform2D`, `Vector4`, `Vector4i`, `Plane`, `Quaternion`, `AABB`, `Basis`, `Transform3D`, `Projection`, `Color`.
    
- Engine types: `RID`, `Callable`, `Signal` (placeholder objects).
    
- `Object`‑derived: `Resource` (with `full_objects=true` → properties; `full_objects=false` → `null`).
    
- `Array`, `TypedArray<int64_t>`, `TypedArray<Resource>`.
    
- `Dictionary` (untyped and typed: `int→int`, `int→Variant`, `Variant→int`, `String→Resource`).
    
- Packed arrays: `PackedByteArray`, `PackedInt32Array`, `PackedInt64Array`, `PackedFloat32Array`, `PackedFloat64Array`, `PackedStringArray`, `PackedVector2Array`, `PackedVector3Array`, `PackedColorArray`, `PackedVector4Array`.
### Out of Scope
- Performance or memory.
    
- JSON schema validation or custom marshalling.
    
- Circular references beyond simple objects.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_json_native.cpp`
## Approach
The test defines helper `encode` and `decode` functions that call `JSON::from_native`/`JSON::to_native` with optional `full_objects` flag. A `test` helper compares the encoded string to an expected literal and verifies that decoding produces a Variant with identical `get_construct_string()` representation. The `TEST_CASE` iterates over all supported types, including edge values (inf, nan) and typed containers. For `Resource` and containers thereof, it runs both with and without `full_objects` and checks error conditions (printed errors temporarily suppressed). All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_json_native.cpp`
