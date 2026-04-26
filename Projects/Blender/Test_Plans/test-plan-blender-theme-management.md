# Test Plan: Blender Theme Management
## Description
This plan covers the functions of installing and managing blender themes through the preferences menu
## Objective
Ensures that the interfaces for installing, switching, and deleting extension themes behave predictably and without errors.
## Scope
### In Scope
- Installing a theme package from the Extensions website (downloaded file)
- Installing a theme from "Get Extensions"
- Switching between different themes (installed via different methods)
- Deleting a theme that was installed from a downloaded file
- Deleting a theme that was installed from "Get Extensions"
### Out of Scope
- Modifying or editing theme colors/sliders
- Creating or exporting themes
- Theme compatibility across different Blender versions
- Performance impact of themes on viewport rendering
## Environment
OS: Ubuntu 22.04 LTS (virtual machine)
CPU: 4 cores AMD Ryzen 5 2400G with Radeon Graphics 3.60 GHz)
RAM: 8 GB allocated
Program Version: Latest source build from the official repository
## Approach
Manual execution. Each test case will be run once with two separate themes: one installed via download file, the other installed from disk.
## Test Cases. 

- [TC-01: Install Theme From File]()
