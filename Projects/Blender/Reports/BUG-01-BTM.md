# BUG-01: Theme Installed via _Get Extensions_ Cannot Be Removed from Themes Panel

**Severity:** Low 
**Date Found:** 2026‑04‑20 
**Date Reported:** 2026‑04‑26 
**Status:** Ongoing

## Environment
- **Application:** Blender (latest build from source as of 2026‑04‑20)
    
- **OS:** Ubuntu 22.04 LTS VM
    
- **Build Type:** Compiled from source

## Prerequisites / Setup
- Successful completion of:
    
    - **TC‑02:** Installing a theme via _Get Extensions_
        
- At least one theme installed through **Get Extensions**
    
- Blender launched using the compiled binary from terminal
## Steps to Reproduce
- Launch Blender using the compiled binary.
    
- On the splash screen, select **General** to open the default file.
    
- Navigate to **Edit → Preferences**.
    
- In the Preferences window, select **Get Extensions** from the left sidebar.
    
- At the top of the Get Extensions panel:
    
    - Open **Filter by Type** → select **Themes**
        
    - Open **Repository** → ensure **extensions.blender.org** is selected
        
- In the search bar, type the name of the desired theme.
    
- In the search results, locate the theme card and click **Install**.
    
- After installation completes, navigate to **Themes** in the left sidebar.
    
- Open the **Theme Selection** dropdown and confirm the newly installed theme appears.
    
- Select the installed theme and verify Blender’s UI updates to match the theme.
    
- Attempt to uninstall the theme:
    
    - Stay in **Preferences → Themes**
        
    - Open the **Theme Selection** dropdown
        
    - Select the theme installed via Get Extensions
        
    - Click the **minus (–)** icon labeled **Remove Theme**
        
    - Confirm the dialog **Remove Custom Theme**

## Expected Result
- The selected theme should be removed successfully.
    
- Blender should immediately revert to the default theme.
    
- No errors or warnings should appear.
    
- The UI should update instantly after deletion.

## Actual Result
- A dialog appears stating: **“Built‑in themes cannot be removed.”**
    
- The theme is not deleted.
    
- Blender incorrectly treats the extension‑installed theme as a built‑in theme.

## Screenshots / Attachments


## Reproducibility
- [X] Always
    
- [ ] Sometimes
    
- [ ] Once only


# Hypothesis
Themes installed via **Get Extensions** are being misclassified internally as **built‑in themes**, rather than user‑installed themes. This results in:

- The Themes panel blocking deletion
    
- The “built‑in” protection logic triggering incorrectly
    
- A mismatch between how Blender handles:
    
    - Themes installed via **Get Extensions**
        
    - Themes installed via **XML file import**
        

Uninstalling the same theme **through the Get Extensions panel** works correctly, which further suggests the Themes panel is not differentiating theme origins properly.

This behavior appears **unintended** and likely stems from incomplete integration between the new Extensions system and the legacy Themes management UI.

## Additional Notes
- The error dialog does not clarify that the theme is extension‑installed, which may confuse users.
	
- This issue is **also reproducible in Blender 5.1.0**, the current stable public release, indicating it is not limited to source builds.
    
- The behavior is consistent across multiple attempts and themes.
    
- No warnings or logs indicating misconfiguration were observed.

# Follow-Up Notes

