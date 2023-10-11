---
title: "PowerShell As Admin Mac OS Shortcut"
summary: A quick how to on how to create a PowerShell Admin shortcut on Mac OS.
date: 2018-10-21
draft: false
categories: ["How To’s"]
tags: ["Azure", "CLI", "How To", "Mac OS", "PowerShell"]
thumbnail: "/img/thumbs/pwsh-macos-thumb.png"
url: "/cli/powershell-as-admin-mac-os-shortcut"
---

This is just a quick article to show you how to create a desktop shortcut to always run PowerShell as admin on Mac OS.

1. Create a text file with the following contents using TextEdit (in plain text):
    ```go {linenos=false}
    #!/bin/bash
    sudo pwsh
    ```
2. Save the file in plain text on your desktop as anything you like, for this example I'll call it PowerShellAdminShortcut.txt

3. You now need to remove the ".txt" file extension from the file if you weren't able to save it without it already, if so skip this step and related sub-steps. To do this, follow the below:
   1. Open Finder
   2. Open Finders' preferences
   3. Select the Advanced tab and the check the box "Show all filename extensions"
   ![Enable Show File Extensions - Mac OS](/img/macos-show-extensions.png)

4. Now rename the file you created at the beginning of this process and remove the file extension “.txt”
![Remove File Extensions - Mac OS](/img/macos-remove-file-extensions.png)

5. Now open the Terminal app and run the following commands (please replace the file path with where you have placed your file):
   ```
   cd /Users/jacktracey/Desktop
   chmod 744 PowerShellAdminShortcut
   ```
   ![chmod on Mac OS](/img/mac-os-chmod.png)

6. You should now have a shortcut on your desktop that launches PowerShell as admin (sudo) on your Mac
   ![PowerShell As Admin Mac OS Shortcut](/img/pwsh-mac-os-shortcut.png)

Hope this helps some of you out there as now all you need to do is double click this shortcut and enter your user account password and job done!