# Test Plan: Image – Instantiation, File I/O, Pixel Manipulation, Mipmaps, and Format Conversion

## Description
This test suite validates Godot's `Image` class, covering creation (empty or with data), saving/loading of various image formats (PNG, EXR, BMP, JPG, WebP, TGA, SVG), basic getters (width, height, size, format, used rect), resizing operations (crop, shrink, resize, power‑of‑two), pixel modifications (set, fill, fill_rect, blend_rect, blit_rect, flip), premultiply alpha, mipmap generation and preservation across format conversions, and conversion between all image formats (including floating‑point and byte formats).
## Objective
Ensure that `Image` can be instantiated correctly (empty vs. sized, data zeroed), saved to and loaded from common image file formats (PNG, EXR, BMP, JPG, WebP, TGA, SVG) with byte‑accurate data equality, that getters return correct values, that resizing and pixel manipulation functions work as expected (bounding rect, color blending vs. replacement, flipping), that mipmaps are generated, stored, and survive format conversions (byte and float), and that conversion between all valid format types succeeds without data corruption.
## Scope
### In Scope
- Instantiation: empty, sized (zeroed data), copy from another image, from existing data buffer.
    
- Saving PNG, EXR (TOOLS_ENABLED only), JPG; loading PNG, BMP, EXR, JPG (including grayscale), WebP, TGA, SVG (embedded JPG).
    
- Basic getters: width, height, size, format, used rect, region extraction.
    
- Resizing: crop, shrink_x2, resize (all interpolation types), resize_to_po2.
    
- Pixel operations: `set_pixel`, `get_pixel`, `is_invisible`, `get_used_rect`, `fill`, `fill_rect` (with various rects, including out‑of‑bounds), `blend_rect`, `blit_rect`, `flip_x`, `flip_y`, `premultiply_alpha`.
    
- Mipmaps: generation, count, offset/size query, preservation after byte format conversion (L8…RGBA8) and float format conversion (RF…RGBAF).
    
- Conversion: round‑trip through all valid `Image::Format` values (RF…RGBE9995), invalid formats do nothing.
### Out of Scope
- Performance, memory, or threading.
    
- Real‑time texture upload or GPU interaction.
    
- Formats beyond those listed (e.g., DDS, KTX).
    
- Error handling for corrupted files (beyond basic load failures).
    
- Writing EXR without `TOOLS_ENABLED`.
## Environment
**OS:** Ubuntu 22.04 LTS (VM)
    
- **CPU:** 4 cores (AMD Ryzen 5 2400G)
    
- **RAM:** 8 GB allocated
    
- **Godot Version:** latest source build
    
- **Execution Method:** Automated C++ file
    
	- **Test Path:** `godot/tests/core/io/test_image.cpp`
## Approach
The test uses Godot’s built‑in doctest framework. Each `TEST_CASE` creates `Image` instances using various constructors, reads/writes test data files via `TestUtils::get_data_path()` and `get_temp_path()`, and validates results with `CHECK_MESSAGE` (and `REQUIRE` for critical steps). Pixel comparisons use `is_equal_approx` with tolerance. Mipmap preservation is checked by writing known pattern values and reading back after conversion. File loading tests are conditionally compiled based on module availability (`MODULE_BMP_ENABLED`, etc.). All tests run automatically via Godot’s `--test` command.
## Test Artifact
- **Automated Script:** `godot/tests/core/io/test_image.cpp`
