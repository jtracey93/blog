---
title: "New Powershell Module Az"
summary: An introduction to the new Azure PowerShell module called 'Az' that will replace the existing 'AzureRM' module.
date: 2018-10-21
draft: false
categories: ["CLI"]
tags: ["Azure", "PowerShell"]
thumbnail: "/img/thumbs/pwsh-logo-thumb.jpg"
url: "/cli/new-powershell-module-az"
---

This week the Azure Software Engineering Team announced a new PowerShell module for Azure called “Az”.

This module is also cross-platform supported which means all commands etc… are supported across all platforms/OS’s that support PowerShell; Windows, Linux, Mac OS X & Azure Cloud Shell.

I have installed this on both my Windows and Mac OS Mojave machines and all seems to be working well and I haven’t found any issues as of yet, which is always nice.

The only small niggle comes when trying to install the module on a Mac if you are not running PowerShell as an admin you get the below error:
![Az PowerShell Mac OS Error](/img/az-mac-os-error.png)

To get around this see my other blog post about how to create a desktop shortcut to launch PowerShell as admin on a mac. [Click here to view that post!](/cli/powershell-as-admin-mac-os-shortcut)

However once you’ve followed the post linked above the install process on a mac is pretty painless.

1. Launch your new PowerShellAdmin executable from the desktop
2. Enter your user account password to launch as sudo
3. Enter the command:
    ```go {linenos=false}
    Install-Module Az
    ```
    ![Install PowerShell Az Module Mac Os](/img/az-pwsh-install-mac-os.png)

4. Accept the PSGallery as an untrusted repository to install from by entering "Y" or "A" at the prompt and hit return (as shown below)
![PowerShell Untrusted Repo](/img/az-pwsh-install-untrusted-repo.png)

5. Let the module download and install
![PowerShell Az Module Installing](/img/az-pwsh-installing.png)

It's as easy as that! 

And the same commands can be followed on a Windows machine with PowerShell running as an administrator.

## Compatibility With The AzureRM Module

Now for those of you, like myself, who have an abundance of PowerShell scripts using the AzureRM module to do various tasks in Azure; I have some further good news for you all.

The team at Microsoft have also thought about this scenario and have provided a couple of commands to assist with this transition period to a new module.

The first command is:

```go {linenos=false}
Enable-AzureRmAlias [-Module <string>] [-Scope Process | CurrentUser | LocalMachine]
```

This enables you to use your legacy AzureRM module references with the new Az module.

Note if you don't specify a specific module then all modules will have the alias enabled. Also you can set the scope for the aliases as you require, I would probably suggest "LocalMachine" for most admins out there until you have re-written your scripts.

The second command is:

```go {linenos=false}
Disable-AzureRmAlias [-Module <string[]>] [-Scope Process | CurrentUser | LocalMachine]
```

This command just disables the aliases that you may have previously enabled; to use once you have adjusted all of your scripts in my opinion.

The same rules around specifying modules and scoping as above.

## Summary

Get out there and install the new module! I can only imagine that the new module will be the only one supported in a few months, so getting ahead of the curve is always a good idea!

Microsoft also mention that the Az module will replace the AzureRM one later this year!

Let me know in the comments if you find any commands that don't work, I haven't found any yet!

And heres to being able to use the same cmdlets across all platforms with this new module!