---
title: "AD DS DCs in Azure"
summary: Active Directory is still a critical service for may of us, even in the Cloud. Have you deployed your DCs correctly?
date: 2019-02-03
draft: false
categories: ["Active Directory"]
tags: ["Azure", "Active Directory", "Availability Set", "Best Practice", "IaaS", "SLA", "Gotchas"]
thumbnail: "/img/thumbs/adds-thumb.png"
url: "/active-directory/ad-ds-dcs-in-azure"
---

This week I received a couple of queries from clients around Active Directory in Azure and more specifically how they should handle/manage their Domain Controller IaaS VMs in Azure.

Now I have seen hundreds of IaaS VMs in Azure as Domain Controllers, it’s something that goes into the majority of my designs for clients today; it’s like a natural reaction/muscle memory for me.

However, these questions from my client made me take a step back and take a deep dive into Active Directory again, something I haven’t done in a few years, and review the recommendations and best practices for running DCs in Azure.

## The Questions…

There were only 2 questions that got me thinking about this topic, they were:

1. Should we place the Active Directory installation (database, logs & SYSVOL) on a separate data disk with caching disabled?
2. Can we shut down the DC IaaS VM from the portal using the stop button as we do for other IaaS VMs?

## The Answers…

So some of you may be thinking that these questions are pretty simple to answer and to some extent you are correct. However taking the time to check and investigate the answers to these questions and application specific best practices from time to time is never a bad idea.

Below I’ll answer each question and break it down as to what you should/shouldn’t do.

### QUESTION 1 – Separate Data Disk For AD DS Data With No Caching

Now this topic isn’t actually specifically related to Azure only, it actually applies to any virtualisation platform (vSphere, Hyper-V, Xen Server, AWS EC2, etc…).

In short the answer is yes, store the AD data on a separate data disk and disable read and write caching.

The why is actually more to do with the caching element of the question. The theory being that if write caching is enabled at the hypervisor level for the data disk (or any disk where AD data is stored for that matter) there is a chance that if the VM is powered off abruptly for any reason, some changes are still waiting to be written/committed to the disk and therefore this can lead to issues like USN rollbacks.

So I would add an additional data disk to your Azure IaaS DC VMs when building them to place the AD install upon.

**Create Managed Disk**
![Creating A Managed Disk](/img/create-managed-disk.png)

**Attach Managed Disk To VM & Disable Caching**
![Attach Managed Disk With No Caching](/img/attach-managed-disk-no-cache.png)

If you have already deployed your DCs and promoted them etc... I would suggest building new ones in Azure with the additional data disk and just following the process to promote/demote DCs. You can migrate the database etc... manually but, why I add the risk when it's so easy to just build new and promote/demote.

### QUESTION 2 - Shutting Down an AD DS DC IaaS VM via Portal Stop Button

So this one is a little more interesting and again isn't exclusively related to Azure and applies to any virtualised DC depending on the hypervisor platform it runs upon.

However keeping this strictly Azure focused, Microsoft advise explicitly on the docs website that you should NOT shut down IaaS DCs via the portal. [You can check that Microsoft page here.](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/adds-extend-domain#manageability-considerations)

Shutting down the VM via the portal causes a chain of events to occur when that VM is eventually powered back on.

The first thing that happens is the VM-Generation ID is reset/changed. The VM-Generation ID is stored as an attribute of the DCs computer object within the AD database called msDS-GenerationID upon promotion.

When a DC is started up and AD is starting up, it checks the VM-Generation ID against the msDS-GenerationID that it has stored in it's database against the DCs computer object. If that value is not the same, the DC resets the Invocation ID and discards its RID pool; adding to the chain reaction!

Thankfully since Windows Server 2012 the VM-Generation ID is supported and stored as an attribute as explained above. So now AD knows exactly what to do when the VM-Generation ID changes to prevent a USN rollback and/or give out duplicate SIDs etc... Clearly none of us want these things to happen, so lets all take a moment and thank Microsoft for this feature since Windows Server 2012.

![Event Viewer ID 2170](/img/event-id-2170.png)

Anyway, now that AD has detected the VM-Generation ID and reset the Invocation ID it will clear the DCs RID pool and update the msDS-GenerationID on the DCs computer object with the new  VM-Generation ID. It will also perform a non-authoritative restore on the affected DC to replicate the SYSVOL and other information from another DC within the domain.

This all happens automatically to ensure that the integrity of the domain stays in tact and no duplicate SIDs are given out and also to keep the replication topology in tact.

![Event Viewer ID 2208](/img/event-id-2208.png)

Regardless that this all happens automatically, it’s still not a healthy thing to be happening to a DC and it is certainly something that can be avoided

So to avoid it all that you need to do is shut down the DC IaaS VM via the guest OS instead of clicking stop in the Azure portal. That’s honestly it. However you can sleep easily knowing that if your DC is at least Windows Server 2012 it will protect you if the VM gets shut down abruptly!

One thing to be aware of when shutting any VM down via the guest OS instead of stopping it via the Azure portal is that the VM will not enter the deallocated state once its shut down. It will just show a status of stopped. This will mean that you will still be charged for the VM compute costs etc… as if the VM was still powered on.

![IaaS VM Stopped Not Deallocated](/img/vm-stopped.png)

Although this does mean that the VM-Generation ID will not change when you start the VM back on!

Another point to consider is that your VM is unlikely to be turned off abruptly and even so you should be deploying at least 2 DCs in an Availability Set using managed disk to give your AD services the best SLA possible from Azure.

## Summary

Again there is a lot of information to take in here. But I feel it’s a vital topic to cover as nearly every deployment will have a IaaS DC in it somewhere. Also to be prepared for the questions above from a client, your boss or an AD specialist is always best, rather than having to research the answer when asked.

Any questions on this topic please leave a comment or drop me a tweet and I’ll happily get back to you.