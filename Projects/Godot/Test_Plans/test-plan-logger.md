# Test Plan: Logger – RotatedFileLogger and CompositeLogger Log Rotation & Composite Logging

## Description
This test suite validates Godot’s `RotatedFileLogger` (which rotates log files when reaching a maximum count) and `CompositeLogger` (which logs to multiple loggers simultaneously). It checks that the first log file is created and contains the expected message, that log rotation renames files correctly and removes the oldest when exceeding the limit, and that a composite logger writes the same message to all child loggers.
## Objective
Ensure `RotatedFileLogger` correctly creates `godot.log` on first write, that rotating after multiple writes produces the expected number of backup files (godot.log, godot.log.1, godot.log.2, etc.) with the expected content order, that exceeding the maximum count removes the oldest backup, and that `CompositeLogger` forwards `logf` calls to every registered logger, writing identical content to each log file.
## Scope
### In Scope
- Single log file creation and content write.
    
- Log rotation with max files = 3: after 3 writes, files `godot.log`, `godot.log.1`, `godot.log.2` exist.
    
- Rotation after 4th write: oldest file removed, new `godot.log` created, others shifted.
    
- Content verification per rotated file (each write contains distinct string `"Waiting for Godot i"`).
    
- Composite logger: two child `RotatedFileLogger` instances, each receives same log message.
    
- Use of `OS::delay_usec()` to ensure filesystem timestamps allow correct rotation.
    
- Setup/cleanup: create `user://logs/` directory, remove all `.log` files after test.
### Out of Scope
- Other logger types (e.g., `StdLogger`).
    
- Log formatting options or log levels.
    
- Performance or concurrency.
    
- Error handling for disk full or invalid paths.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_logger.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. A helper `initialize_logs()` sets the project name and creates the `user://logs/` directory. Each `TEST_CASE` creates loggers, writes messages, uses `delay_usec` to force new timestamps for rotation, then reads back files via `FileAccess` and compares their content. Another helper `get_log_files()` collects rotated log file names (excluding `godot.log` and then adding it last). `cleanup_logs()` deletes all created files and directories. All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_logger.cpp`
