# Test Plan: IP – Hostname Resolution for Localhost

## Description
This test validates that Godot's `IP` singleton correctly resolves the hostname `"localhost"` to its IPv4 and IPv6 addresses. It repeatedly calls `resolve_hostname` 1000 times and checks that the returned `IPAddress` string matches `"127.0.0.1"` for IPv4 and `"0:0:0:0:0:0:0:1"` for IPv6.
## Objective
Ensure that `IP::resolve_hostname` consistently returns the correct loopback addresses for `"localhost"` across multiple calls (no caching or threading issues). The test verifies both IP versions (`TYPE_IPV4` and `TYPE_IPV6`) return the expected strings.
## Scope
### In Scope
- Resolving `"localhost"` with `IP::TYPE_IPV4` → expects `"127.0.0.1"`.
    
- Resolving `"localhost"` with `IP::TYPE_IPV6` → expects `"0:0:0:0:0:0:0:1"` (standard IPv6 loopback).
    
- 1000 iterations to stress resolution consistency.
### Out of Scope
- Other hostnames or domain resolution.
    
- Network failures or DNS timeouts (assumes `localhost` resolves instantly).
    
- Performance or memory usage.
    
- Asynchronous resolution (uses blocking call).
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_ip.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Inside the `TEST_CASE`, a loop runs 1000 times, calling `IP::get_singleton()->resolve_hostname("localhost", type)` for both `IP::TYPE_IPV4` and `IP::TYPE_IPV6`. Each result is converted to a `String` and compared with the expected literal using `CHECK`. The test runs automatically via Godot’s `--test` command (requires network stack but does not actually go online; `localhost` resolution uses the local loopback interface).
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_ip.cpp`
