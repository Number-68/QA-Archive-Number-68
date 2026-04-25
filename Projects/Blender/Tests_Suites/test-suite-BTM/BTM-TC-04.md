# TC - 04: Uninstall Theme Installed from Downloaded File

# Prerequisites:
- Successful completion of **Test Case 1**.
    
- Successful completion of **Test Case 3**
        
- A successfully built Blender application with the binary accessible.

# Test Steps:
- **Launch Blender** from the terminal using the compiled Blender binary.
    
- When the splash screen appears, click **General** to open a new default project.
    
- In the top-left application menu bar, click **Edit**.
    
- From the dropdown menu, select **Preferences**.
    
- In the Preferences window, select **Themes** from the left sidebar.
    
- Open the **Theme Selection** dropdown at the top of the Themes panel.
    
- Select the theme that was installed via **downloaded XML file** (from Test Case 1).
    
- Locate the **minus (“–”) icon** next to the theme controls (Remove Custom Theme).
    
- Click the **minus icon**.
    
- A confirmation dialog labeled **Remove Custom Theme** should appear.
    
- Click **Delete** to confirm removal.

# Expected Results:
- The selected custom theme should be **successfully removed**.
    
- Blender should immediately revert to a **default theme** (e.g., Blender Light or Blender Dark).
    
- The removed theme should **no longer appear** in the Theme Selection dropdown.
    
- No errors, warnings, or unexpected behavior should occur.
    
- The UI should update instantly after deletion.