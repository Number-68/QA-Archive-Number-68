# Test Plan: HTTPClient – Instantiation, Query String Construction, Header Validation, and Host Connection

## Description
This test suite validates Godot's `HTTPClient` class for basic HTTP operations. It checks that a client can be instantiated successfully, that `query_string_from_dict` correctly converts dictionaries into URL‑encoded query strings (handling strings, integers, arrays, and null values), that `verify_headers` accepts valid headers and rejects empty or malformed headers (missing colon, colon in first position), and that `connect_to_host` can establish a connection to a remote server when TLS support is enabled.
## Objective
Ensure that `HTTPClient::create()` returns a valid reference, that query string generation produces the expected format (key=value, & separator, arrays expanded to multiple key=value pairs, null values become key only), that header validation returns `OK` only for properly formatted headers and `ERR_INVALID_PARAMETER` for invalid ones, and that `connect_to_host` with HTTPS and default TLSOptions succeeds when TLS is available (mbedtls or Web).
## Scope
### In Scope
- Instantiation: `HTTPClient::create()` returns a non‑null `Ref<HTTPClient>`.
    
- `query_string_from_dict`: empty dict, single key‑value string, mixed types (string, int, array, null).
    
- `verify_headers`: valid headers, empty header string, header without colon, colon‑only header.
    
- `connect_to_host` (only when `MODULE_MBEDTLS_ENABLED` or `WEB_ENABLED`): connect to `https://www.example.com:443` with default TLS options, expecting `OK`.
### Out of Scope
- Actual HTTP requests (GET, POST, etc.) or response handling.
    
- Connection timeouts or error recovery.
    
- Custom TLS options or certificate validation.
    
- Non‑HTTPS connections.
    
- Performance or network reliability.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_http_client.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` creates or uses a client instance. Query tests compare the returned string against expected literals. Header tests suppress error prints while checking error codes. The `connect_to_host` test is conditionally compiled when TLS support is available. All tests run automatically via Godot’s `--test` command; network connectivity is required for the connection test (it assumes `example.com` is reachable).
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_http_client.cpp`
