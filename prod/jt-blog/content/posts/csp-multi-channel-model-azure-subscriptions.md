---
title: "CSP Multi Channel Model Azure Subscriptions"
summary: Explaining the lesser known details about Azure Subscriptions from the CSP platform.
date: 2019-05-11
draft: false
categories: ["CSP"]
tags: ["Azure", "CSP", "Gotchas", "Licensing", "Migration", "Subscriptions"]
thumbnail: "/img/thumbs/csp-thumb.png"
url: "/csp/csp-multi-channel-model-azure-subscriptions"
---
{{< rawhtml >}}

<img src="/img/csp-diagram.png" align="right">

{{< /rawhtml >}}

A slightly less technical log post for today’s topic, however a vital one for anyone using/thinking of using an Azure CSP Subscription.

Firstly a quick explanation of the CSP (Cloud Solution Provider) program.

CSP is a program that partners can sign up to in order to sell Microsoft Cloud Services like Azure, Office 365, Microsoft 365, EM+S and other similar services to their customers directly; instead of customers having to purchase them directly from  Microsoft.

CSP has had a massive uptake over the last 3-5 years and pretty much every MSP (Managed Service Provider) has access to a CSP program, whether directly or indirectly, to sell Microsoft Cloud services to their customer.

The key fact around CSP is that the CSP partner you chose to purchase Office 365 or Azure subscriptions from etc… is responsible for billing the client and providing support for both billing and technical support. Meaning the CSP partner effectively becomes the single point of contact for the client for all things relating to the Microsoft Cloud.

Further information around CSP can be found here: [https://docs.microsoft.com/en-us/azure/cloud-solution-provider/overview/azure-csp-overview](https://docs.microsoft.com/en-us/azure/cloud-solution-provider/overview/azure-csp-overview)

I’ll be blogging soon about things you should consider before choosing between CSP and other subscriptions types for Azure (EA/PAYG etc…). So watch out for that.

However this post focuses on a particular limitation of CSP Azure Subscriptions in a Multi-Channel Model approach that is commonly not known about and can be a road block to project flows if not known about.

## What is the CSP Multi-Channel Model?

When CSP first launched it only allowed the provisioning CSP partner to add/remove/amend licenses and subcriptions for each customer. So if you had a scenario where the customer, “ACME Corporation”, purchased 10 X Office 365 E3 licenses from CSP Partner 1; Only CSP Partner 1 could sell them additional licenses or provision any other services available in the CSP program, like Azure etc…

This obviously locked customers like, “ACME Corporation”, into a relationship with CSP Partner 1 until the end of time. Both customers and other CSP partners didn’t like this as effectively people where just getting any customer to sign up for CSP for a tiny amount of services to lock them in for future consumption costs.

Microsoft then released the CSP Multi-Channel Model which allowed another partner, say CSP Partner 2 in our above scenario, to start a CSP relationship with “ACME Corporation” which would allow them to purchase CSP services from both CSP partners at the same time; even for the same license SKU types.

For example “ACME Corporation” could have 10 x Office 365 E3 licenses from CSP Partner 1 & then start another relationship with CSP Partner 2 and purchase and additional 5 x Office 365 E3 licenses from them also. In this scenario each partner bills “ACME Corporation” for 10 x Office 365 E3 licenses separately and everyone is happy.

Also an important note is that when another relationship is established with another CSP partner they cannot amend and services/licenses provided by another other CSP partner to the same customer; very handy indeed!

## So why doesn’t this work in the same way for Azure subscriptions?

Unfortunately for Azure subscriptions the CSP Multi-Channel model doesn’t allow 2 CSP partners to each provide Azure subscriptions to a customer.

So in our example scenario above, “ACME Corporation” could be provided Office 365 licenses from both CSP partners at the same time but only 1 CSP partner could provide an Azure subscription at any one time.

This can be quite a road blocker when not known about! It is certainly 1 thing I have had to stop from occurring multiple times in the last year or so and it’s becoming ever more popular as more customer take services from CSP partners.

There is a note on the following [Microsoft Docs page](https://docs.microsoft.com/en-us/azure/cloud-solution-provider/customer-management/add-existing-customer) for adding existing Azure customers in CSP that is often not missed and not read and understood in full.

The note is shown below:
![CSP Azure Multi Partner Note ](/img/csp-multi-partner-note.png)

## Is there a way around this?

There is but its not simple and takes considerable thought and consideration before starting the process.

The only way around this is to transfer the Azure services between CSP partners. This does mean the source partner will lost all billing and revenue from the Azure subscription once the transfer process is complete. However if they are providing Office 365 or any other license model based services via CSP these are not transferred during the Azure transfer process. These are in-fact very easy to switch CSP Partner 2 provisions all the same quantities and licenses for each SKU in use by the customer then CSP Partner 1 cancels all those licenses fort the customer and then each users licenses automatically flips over the the licenses provided by CSP Partner 2.

The Azure transfer process is documented fully by Microsoft here: [https://docs.microsoft.com/en-us/partner-center/switch-azure-subscriptions-to-a-different-partner](https://docs.microsoft.com/en-us/partner-center/switch-azure-subscriptions-to-a-different-partner)

The process requires involvement from both CSP partners, the customer and Microsoft support to complete the process in full. So a good relationship with all parties involved is vital to ensure a smooth transfer process!

## Summary

I hope this helps customers and other CSP Partners out there as this is certainly going to become more of a day to day occurrence as more customers utilise CSP partners for Azure subscriptions.

Look out for more CSP related articles soon!