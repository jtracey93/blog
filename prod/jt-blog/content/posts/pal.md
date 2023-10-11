---
title: "Partner Admin Link (PAL) PowerShell Script"
summary: A brief summary of Partner Admin Link (PAL) which is a must for Microsoft Partners working with customers on Azure!
date: 2019-09-19
draft: false
categories: ["Scripts"]
tags: ["Azure", "CSP", "MPN", "PAL", "Partner", "PowerShell", "Script"]
thumbnail: "/img/thumbs/pwsh-logo-thumb.jpg"
url: "/scripts/pal"
---
{{< rawhtml >}}

<img src="/img/msft-partner-logo.png" width="40%" align="right">

{{< /rawhtml >}}

***Update 21/09/2019 –  [PowerShell Script V2 Released](https://github.com/jtracey93/PublicScripts/blob/master/Azure/PowerShell/Partner%20Tools/LinkMPNPartnerIDToAzureAccountV2.ps1)***

Just a quick post today to share a new tool I have created for all Microsoft Partners out there who are helping customers design, build, manage and operate Azure.

## Partner Admin Link (PAL) Overview

Partner Admin Link (PAL) is a method for partners to associate themselves to customers Azure environments, to enable them to associate themselves to that customer Azure Consumed Revenue (ACR – not Azure Container Registry this time).

A blurb from Microsoft on PAL is below:

**What is Partner Admin Link?**

*"Partner Admin Link (PAL) is designed for managed service providers (MSPs). Assuming the MSP has access to resources in the customer subscription then they can link their those accounts to their MPN ID. From that point onwards the telemetry for those resources (and only those resources) will be linked to the partner."*

## Methods To Setup Partner Admin Link (PAL)

There are 3 ways to configure PAL on a customers Azure environments:

- Via The Azure Portal
- PowerShell
- AZ CLI
- All of which are documented nicely over in the [Microsoft Docs.](https://docs.microsoft.com/en-us/azure/billing/billing-partner-admin-link-started)

## My Handy PowerShell Script

I created this script for use at the company I work for, as we need to ensure this is done every time for every user when logging into a new customers Azure environment for the first time.

However, I couldn’t not share it with the community as this will likely help a lot of you out there. Also it was really nice to get some more hands on time with VS Code, Windows Terminal and PowerShell again; it’s been a couple of weeks due to mainly being in meetings etc… and no hands-on time.

Anyway, the script is available via my [GitHub](https://github.com/jtracey93) account in my [PublicScripts](https://github.com/jtracey93/PublicScripts) repository.

The get access directly to the script in the repository on my GitHub account, [click here.](https://github.com/jtracey93/PublicScripts/blob/master/Azure/PowerShell/Partner%20Tools/LinkMPNPartnerIDToAzureAccountV2.ps1)

As always please create issues or submit pull requests for any issues with the script  or anything you’d like changed. I will review them as they come in.

Finally, there are no guarantees for the functionality of this script. I have tested it several times in different Azure environments and it has worked perfectly. But please use at your own risk.

## Things To Look Out For With Partner Admin Link (PAL)

Here are some quick tips, tricks and pointers about PAL that I have discovered and learnt. *(I will update the Microsoft Docs pages as well if these aren’t posted over there too)*

1. You don’t need to do this on CSP Azure Environments as the ACR is already tracked automatically for you.
2. PAL is linked on a per user, per tenant basis.
   - With this in mind it is advised to make sure all of your employees with access to customer subscriptions should setup and configure PAL on each customer they have access to.
3. You can have any RBAC role assigned to setup and configure PAL on a user account in a customers Azure Environment, even as low as ‘Reader’.
   - This is because even having ‘Reader’ rights shows the customer has placed trust in you as a partner to assist them.
