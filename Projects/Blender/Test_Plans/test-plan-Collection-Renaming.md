# Test Plan: View Layer – Collection Renaming and Duplicate Name Handling

## Description
This test validates the naming rules for collections in Blender's view layer system. It creates a hierarchy of collections (grandma, grandpa, mom, son, daughter, uncle, cousin) and tests that renaming a collection to match an existing name is allowed only when the two collections are not siblings (i.e., they belong to different parents). It also verifies that adding a new collection with the same name as an existing sibling is automatically disambiguated, while adding a new collection with the same name as a non‑sibling is allowed.
## Objective
Ensure that collection names must be unique within the same parent (siblings), but can duplicate across different branches of the hierarchy. The test checks that renaming a collection to an existing sibling name results in a changed name (automatically disambiguated), while renaming to a non‑sibling name keeps the duplicate. Similarly, adding a new collection with the same name as an existing sibling produces a different name (e.g., "name.001"), whereas adding under a different parent preserves the duplicate name.
## Scope
### In Scope
- Creating a hierarchy with 7 collections (grandma, grandpa, mom, son, daughter, uncle, cousin).
    
- Renaming tests:
    
    - Rename a collection to the name of a non‑sibling → duplicate allowed (names equal).
        
    - Rename a collection to the name of a sibling → automatic disambiguation (names not equal).
        
- Adding a new collection with the same name as an existing sibling → automatic disambiguation.
    
- Adding a new collection with the same name as an existing non‑sibling → duplicate allowed (names equal).
### Out of Scope
- UI interactions (operators called directly).
    
- Performance or memory.
    
- Collection deletion or moving.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Blender Version:** 5.2.0 Alpha (latest source build)
    
- **Execution Method:** Automated Python test
    
	- **Test Path:** `blender/tests/python/view_layer/test_collection_rename_a.py`
## Approach
The test uses the custom `ViewLayerTesting` base class (extending `unittest.TestCase`). A `setup_family()` helper creates the collection hierarchy and returns a name‑to‑collection lookup dictionary. Each test method renames a collection or adds a new collection, then asserts whether the names are equal or not based on sibling relationship. The test runs headlessly via Blender’s background mode.
## Test Artifact
- **Automated Script:** `blender/tests/python/view_layer/test_collection_rename_a.py`
