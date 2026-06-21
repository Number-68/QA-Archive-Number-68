# Test Plan: XMLParser – XML Document Parsing, Node Types, and Error Handling

## Description
This test suite validates Godot's `XMLParser` class, which parses XML documents. It tests end‑to‑end parsing of a simple XML document, extracting node types (unknown, element, element end, text, comment, CDATA), attribute names and values, and text content with entity decoding (`&lt;`, `&#65;`, `&#x42;`). It also tests comment handling (including missing delimiters, bad starts, unbalanced angle brackets, doctype), CDATA sections, and robust handling of premature endings (incomplete tags, attributes, CDATA, text) without crashing.
## Objective
Ensure that `XMLParser` correctly parses valid XML documents, identifies all node types and their data, decodes HTML/XML entities, handles comments and CDATA blocks as expected, and gracefully handles incomplete or malformed XML input (returning partial data without crashing). Verify that attribute counts and values are correctly extracted, and that edge cases (unclosed comments, incomplete start/end tags, trailing text) do not cause errors.
## Scope
### In Scope
- Parsing a full XML document with prolog, element, attributes, text with entities.
    
- Node type detection: `NODE_UNKNOWN`, `NODE_ELEMENT`, `NODE_ELEMENT_END`, `NODE_TEXT`, `NODE_COMMENT`, `NODE_CDATA`.
    
- Attribute extraction: count, name, value.
    
- Entity decoding: `&lt;` → `<`, `&#65;` → `A`, `&#x42;` → `B`.
    
- Comment parsing: valid comments, missing end (`<!-- foo`), bad start (`<!-`), unbalanced angle brackets (`<!-- << -->`), doctype (`<!DOCTYPE ...>`).
    
- CDATA parsing: content extraction.
    
- Premature endings: incomplete unknown (`<?xml`), incomplete CDATA (`<![CD`), incomplete start‑tag name (`<second`), incomplete attribute name/value, incomplete end‑tag name (`</fir`), trailing text.
### Out of Scope
- XML namespaces or schema validation.
    
- Large file parsing performance.
    
- Encoding issues beyond UTF‑8.
    
- XPath or DOM manipulation.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_xml_parser.cpp`
## Approach
The test uses Godot's built‑in doctest framework. Each `TEST_CASE` creates an `XMLParser`, opens a UTF‑8 buffer with the XML source, and calls `read()` repeatedly, checking node types and data with `CHECK` and `CHECK_EQ`. Entity decoding is verified by comparing decoded text. Comment and premature‑ending tests use `SUBCASE` blocks to test multiple scenarios. Error cases are not expected to crash; the parser returns `OK` and may produce partial nodes. All tests run automatically via Godot's `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_xml_parser.cpp`
