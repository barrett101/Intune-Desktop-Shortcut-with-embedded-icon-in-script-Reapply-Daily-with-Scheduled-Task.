# Intune-Desktop-Shortcut-with-embedded-icon-in-script-Reapply-Daily-with-Scheduled-Task.

## Description
This is similar to the "Intune-Desktop-Shortcut-with-embedded-icon-in-script" repository but it will create the icon, and rerun every 24 hours to ensure the icons is still there.

This powershell script will create a shortcut (.lnk or .url) with a custom icon that is generated by using the Base64 embedded string within the script (no icon file required).  It is designed to run as an Intune Platform script.  When this script is run it will create a scheduled task, and generate supporting PS1 and VBS scripts to ensure the shortcut is always present for the user assigned.  The schedule task will run daily, and if the scheduled start time is missed it will run ASAP.  This script will create a scheduled task, a folder, ShortcutCreator.ps1, ShortcutSilentRunner.vbs, and a log file.  When the scheduled task is trigger it will run completely silent thanks to the VBS script triggering the PS1.  Please note the sub script that creates the shortcut may be hard to read in an Editor.  See ScriptLog.log in the path defined in the "FolderStore" variable.  

IMPORTANT:  THIS SCRIPT IS FOR USER ASSIGNMENT ONLY, NOT TESTED APPLYING TO A COMPUTER object.  If you need it create a shortcut on the all user desktop then use V1 of the script and point to C:\Users\Public profile.  The scheduled task may not work properly in a computer assignment situation.
    
Inspired By and Assistance from:  https://community.spiceworks.com/t/powershell-shortcut-with-custom-embedded-icon/808892
    
### Removing Shortcuts
If you need to delete shortcuts modify this script "Action" and "ScriptMode" variable to "Remove", upload the script, make note in the Intune description when you set to delete, and leave the assignments in place.  Plan to fully delete the Platform script in 6 months down the road, this give your computers time to run the script and cleanup the shortcut.

### Adjusting URL
If you just need to update the URL only, set the "Action" and "ScriptMode" variable to "Add", adjust the ShortcutUrl variable, and then upload the script.  It will run one time again since a new script was uploaded, and update all the supporting scripts with the new URL.

### Changing Shortcut Name
If you just need to update the Name of the shortcut, you will need to make a new Platform script.  On the existing Platform script you will need to go through the "Removing Shortcuts" section above, and keep the assignments in place so it removes everything..

### Adjusting the Desktop shortcut folder location (ex. Putting shortcut in sub folder on desktop)
Adjust the "Desktop" variable as needed, but make sure to keep the "$([Environment]::GetFolderPath("Desktop"))" see below in comments for how to add a sub folder if required.  If removing a shortcut and the sub folder is empty after deleting of the shortcut it will delete the sub folder.

### Turning off icon
You may not want to have an icon file, if not adjust the "EnableIcon" variable to $False.  Use the commented out line.

### Step by Step
1. Set the "TaskName" variable to "SHORTCUT - .....", this will be the name of the folder and scheduled task, use a unique name.
2. Set the "FolderStore" variable to a location in the user profile, the "SetTaskName" variable must be part of the variable.
3. Adjust the "ScriptMode" variable to either "Add" or "Remove", this must match the "Action" variable value.
4. Create or locate an .ICO file, if needed convert an image to ICO using https://convertio.co/jpg-ico/
5. "Action" variable to either "Add" or "Remove", this must match the "ScriptMode" variable value.
6. Upload the .ICO file to https://base64.guru/converter/encode/image/ico and then copy the Base64 text and replace it below in the $IconBase64 variable
7. Create or locate an .ICO file, if needed convert an image to ICO using https://convertio.co/jpg-ico/
8. Adjust the "ShortcutName".
9. Adjust the "ShortcutPath".
10. Adjust the "ShortcutType" to either .url or .lnk.
11. Adjust the "Desktop" path you want the shortcut to be located.  Recommended to keep default, see notes.
12. Adjust the "EnableIcon" to $True or $False depending if you want an icon or not.
13. Create a Platform Script to push out.
