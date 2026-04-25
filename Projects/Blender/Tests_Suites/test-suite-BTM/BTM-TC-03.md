# TC - 03: Switching Between Installed Themes
# Prerequisites:
- Successful completion of **Test Case 1** (Install Theme from Downloaded File).
    
- Successful completion of **Test Case 2** (Install Theme via Get Extensions).
    
- At least **two installed themes**:
    
    - One installed via **downloaded XML file**
        
    - One installed via **Get Extensions**
        
- Blender is successfully built and launchable.

# Test Steps:
- **Launch Blender** from the terminal using the compiled Blender binary.
    
- When the splash screen appears, click **General** to open a new default project.
    
- In the top-left application menu bar, click **Edit**.
    
- From the dropdown, select **Preferences**.
    
- In the Preferences window, select **Themes** from the left sidebar.
    
- At the top of the Themes panel, open the **Theme Selection** dropdown.
    
- Select **Blender Light** (default theme).
    
- Select **Blender Dark** (default theme).
    
- Select the theme installed from the **downloaded XML file** (from Test Case 1).
    
- Select the theme installed via **Get Extensions** (from Test Case 2).

# Expected Results:
- Blender should switch between all selected themes **smoothly and immediately**.
    
- The UI should update to match each theme without delay.
    
- No error messages, warnings, or failures should occur during theme switching.
    
- All installed themes (default, downloaded, and Get Extensions) should appear in the dropdown and be selectable.