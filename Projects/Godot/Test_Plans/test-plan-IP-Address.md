# Test Plan: IPAddress – IPv4/IPv6 Validation, Parsing, and Wildcard Handling

## Description
This test suite validates Godot's `IPAddress` class for correctness of IP address parsing and validation. It checks that valid IPv4 and IPv6 addresses (including compact notation, IPv4‑mapped IPv6, wildcard `*`) are accepted and correctly parsed into binary form, while malformed addresses (spaces, out‑of‑range octets, extra colons, invalid characters) are rejected. The test also verifies the `is_valid()`, `is_wildcard()`, and conversion to/from string.
## Objective
Ensure that `IP::resolve_hostname` consistently returns the correct loopback addresses for `"localhost"` across multiple calls (no caching or threading issues). The test verifies both IP versions (`TYPE_IPV4` and Ensure that `IPAddress` correctly distinguishes between valid and invalid IPv4/v6 strings, parses addresses into the expected binary representation (network byte order), identifies the wildcard address `"*"` as wildcard, and rejects any string that does not conform to the respective IP format (including leading/trailing spaces, extra dots/colons, out‑of‑range values).) return the expected strings.
## Scope
### In Scope
- IPv4 validation: `127.0.0.1`, `0.0.0.0`, `255.255.255.255` (valid); invalid cases (negative, >255, extra dots, spaces, empty).
    
- IPv4 parsing: correct 32‑bit binary result (e.g., `127.0.0.1` → `2130706433`).
    
- IPv6 validation: compact notation (`::1`, `1::`, `::`, IPv4‑mapped `::ffff:127.0.0.1`), uppercase hex allowed, maximum 8 segments; invalid cases (more than 8 segments, double `::`, invalid characters, trailing/leading spaces).
    
- IPv6 parsing: correct 128‑bit binary result (16 bytes, network order).
    
- Wildcard: `"*"` recognized as wildcard (`is_wildcard() == true`, `is_valid() == false`).
    
- Miscellaneous: empty string, spaces only, `"not an ip"` → invalid.
### Out of Scope
- DNS resolution (handled by `IP` class).
    
- IPv6 zone indices (`%eth0`).
    
- Performance or memory.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_ip_address.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. A helper struct `IPTester` exposes static methods to call private parsing functions (`_parse_ipv4`, `_parse_ipv6`). Each `TEST_CASE` iterates over predefined strings, calls `test_v4`/`test_v6`/`test_wildcard`, and asserts the boolean result. Parsing tests compare the binary output of `IPAddress(l_ip).get_ipv4()`/`get_ipv6()` against expected values (converted to network byte order). All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_ip_address.cpp`
