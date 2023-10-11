---
title: "Azure Resource Locks the What Why"
summary: Worried about resources in Azure being accidentally deleted? You need to read this post!
date: 2019-07-29
draft: false
categories: ["Governance"]
tags: ["Azure", "Best Practice", "Compliance", "Governance", "Lock", "Security", "Subscriptions"]
thumbnail: "/img/thumbs/padlocks-thumb.jpg"
url: "/governance/azure-resource-locks-the-what-why"
---
{{< rawhtml >}}

<img src="/img/res-locks-icon.png" align="right">

{{< /rawhtml >}}

Today I want to give you all an overview of Azure Resource Locks. Firstly about what they are and can do, then secondly how you can use them and some best practices around them. And finally a few things to watch out for so you don’t get caught out when using them; believe me it’s easily done!

## What are Azure Resource Locks?

Resource Locking within Azure provides a method to lock subscriptions, resource groups or individual resources to protect them from accidental deletion and changes; even for administrators (depending on their RBAC role).

Resource Locks come in 2 levels; CanNotDelete (**displayed as ‘Delete’ in the portal**) or ReadOnly (**displayed as ‘Read-only’ in the portal**).
![Resource Locks In Portal](/img/res-locks-portal.png)

As the names suggest, CanNotDelete means the resources with the lock applied can be read and modified, but they cannot be deleted. The ReadOnly lock means that the resources with the lock applied can only be read but not modified or deleted.

Both types of lock can actually cause some resources to stop functioning as you’d expect so be aware. More on this later!

Resource locks can also only be created by users assigned the ‘Owner’ or ‘User Access Administrator’ RBAC roles. More specifically any users with roles that grant the following RBAC permissions: 
{{< rawhtml >}}
 <p>‘Microsoft.Authorization/*’ or ‘Microsoft.Authorization/locks/*’</p>
{{< /rawhtml >}} 

So you don’t need to worry about anyone with write permissions on Resource Groups or VMs etc… creating locks on everything and potentially breaking other services without knowing about it etc…

## How should I use Azure Resource Locks?

Now you may think that once deployed putting ReadOnly locks on everything is probably the best thing to go and do. And in some select scenarios I may even agree with you, however this is not best practice; so only do this if you absolutely have too!

{{< rawhtml >}}

<img src="/img/az-gov-scopes-old.png" width="40%" align="right">

{{< /rawhtml >}}

Resource Locks can be applied at the following Azure governance scoping levels:

- Subscription
- Resource Group
- Resource

In my opinion, applying locks at the Subscription level is way to high in the governance hierarchy structure and makes using Azure clunky; as everything you deploy within that subscription will inherit the lock from the subscription level.

Taking it to the other end of the governance hierarchy at placing locks at the Resource level can again be very tedious and difficult to manage and keep on top of. However in some cases it can actually be very useful.

Think of a scenario where you have applied a CanNotDelete lock at the resource group level but on the VPN gateway you have deployed within the resource group you want to restrict anyone from changing anything about its configuration. Applying a ReadOnly lock on the VPN gateway resource as well will mean that the rest of the resource group’s resources can be modified as they need to be but the VPN gateway is completely locked and cannot be modified at all without the lock being removed first. Removing the lock itself requires specific permissions, as explained in the above section, so this become a very handy way of locking things down further.

It should also be stated that if 2 locks, 1 in CanNotDelete mode & 1 in ReadOnly mode, **the most restrictive lock (ReadOnly) takes precedence.**

So the only scope we haven’t mentioned yet is the Resource Group level. This is where I suggest the majority of your locks are applied. However this does rely on you having split resources in some fashion between multiple resource groups. Whether that’s by application, business unit or service; as long as they are split and not all in a single resource group this approach will work nicely!

Applying locks at the Resource Group level is also the advised best practice from Microsoft under the Enterprise Scaffold framework (no part of the Cloud Adoption Framework). And as explained in the above scenario its give you the best flexibility for control without the locks becoming a restricting factor in using Azure on a daily basis.

When governance controls become a blocker for using and consuming Azure you’ll find people will try as many ways as possible to avoid following the controls you have put in place. So my advice is to use them as guard rails and not super secure enforcement rules to stop this from happening.

## How do I create and apply Azure Resource Locks?

This is actually quite easy and I find doing it via the Azure Portal is the best way as you can check what resources will be impacted very easily & quickly due to the visual nature of the portal. However locks can be applied via PowerShell, AZ CLI, ARM Templates & Terraform etc…

In the below example I will place a ReadOnly lock on my test resource group which contains a single storage account.

1. Log into the portal and select the Resource Group you wish to apply a lock to
   ![Resource Lock Step 1](/img/az-res-lock-step-1.png)

2. Select the ‘Locks’ blade from the navigation bar on the right
   ![Resource Lock Step 2](/img/az-res-lock-step-2.png)
3. Click 'Add' on the right blade in the portal
    ![Resource Lock Step 3](/img/az-res-lock-step-3.png)
4. Fill out the details as required and select the ‘Lock Type’ then click ‘OK’
   ![Resource Lock Step 4](/img/az-res-lock-step-4.png)
5. The lock will now be applied – *Note the ‘Scope’ is shown in the lock overview blade as well as ‘Notes’. Make sure you use notes as it helps the next person when they come across your locks.*
   ![Resource Lock Step 5](/img/az-res-lock-step-5.png)
6. That is it. You can follow the same instructions and a Subscription or Resource level and the steps are the same.

If we now look at the Storage Account in this resource group we will see it has inherited the lock.
![Resource Lock In Place](/img/az-res-lock-complete.png)

If I now try to amend the storage account you will see that even though I’m an owner on the Subscription the lock still applies to me and I must manually remove it in order to amend the storage account or delete it..
![Resource Lock Delete Fail](/img/az-res-lock-delete-fail.png)

## Things to watch out for with Azure Resource Locks

As mentioned at the beginning of this blog post, using resource locks can actually break some functionality with resources.

The ones I know of to date are as follows:

1. Listing Storage Account Access Keys
   ![Resource Lock Storage Account Keys Issue](/img/az-res-lock-sac-keys.png)

   This happens whether using the Portal, PowerShell, AZ CLI etc… as they all utilise the Azure Resource Manager (ARM).
2. Azure Backup of VM’s Managed Disks
   ![Resource Lock Managed Disk Backup Fail](/img/az-res-lock-disk-backup-fail.png)

   (Thanks to Adin Ernie for the screenshot of this as I didn’t have one to hand at time of writing.)
    This occurs because the ‘RestorePointCollection’ object is treated as a separate Resource object by ARM. So you cannot place any locks on Resource Groups that contain these objects. The resource group name will look like this: AzureBackupRG_ukwest_1 However the region name will change depending on where you are deploying resources and backing up etc…

    ![Resource Explorer Backup RSG](/img/az-res-lock-disk-backup-fail-res-exp.png)

## Summary

Hopefully this article has given you an in depth overview of Azure Resource Locks and how they can be and should be used.

Further reading can be found on the [Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-lock-resources) pages as always.