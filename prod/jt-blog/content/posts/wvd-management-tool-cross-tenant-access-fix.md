---
title: "WVD Management Tool – Cross Tenant Access Fix"
summary: Have you deployed the WVD Management Tool but need to be able to access a WVD deployment in another AAD Tenant? Read this!
date: 2019-09-30
draft: false
categories: ["WVD"]
tags: ["App Registration", "Authentication", "Azure", "Azure AD", "Azure AD Connect", "Subscriptions", "Windows Virtual Desktop", "WVD"]
thumbnail: "/img/thumbs/wvd-thumb.png"
url: "/wvd/wvd-management-tool-cross-tenant-access-fix"
---
{{< rawhtml >}}

<img src="/img/wvd-logo.png" width="35%" align="right">

{{< /rawhtml >}}

With the [announcement](https://www.microsoft.com/en-us/microsoft-365/blog/2019/09/30/windows-virtual-desktop-generally-available-worldwide/) of WVD (Windows Virtual Desktop) moving into GA today I thought I’d spin my lab back up again and try and get the [WVD Management Web App Tool](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux) working.

My last attempt a few weekends ago was unsuccessful and I didn’t have any time to carry on troubleshooting so I deleted the Resource Group and forgot about it the long To-Do list for another day.

So this evening as the working day was nearing it’s end, I headed back into the Azure portal and attempted to deploy the WVD Management Tool.

## My WVD Setup – Azure AD, Subscriptions & Resources
![JT WVD Setup Overview](/img/thumbs/wvd-jt-ovw-diag.png)

{{< rawhtml >}}

<img src="/img/homer-doh-computer.jpg" width="30%" align="right">

{{< /rawhtml >}}
## Issue #1 – Azure Automation Job Failing

This was a pretty simple one in the end. The Azure AD user’s password I was giving in the ARM template parameters had expired.

This user needs to be an Azure AD Global Administrator and have the RBAC permissions on the Subscription you want to deploy the WVD Management Tool too.

{{< rawhtml >}}

<br>

{{< /rawhtml >}}

## Issue #2 – My Subscription & Azure AD Tenant Aren’t Linked

Now most people will have a single Azure AD Tenant and all their users synced to this and also all of their Azure Subscriptions linked to the same tenant.

However I do not, for a number of reasons, mainly for giving me the opportunity to test a lot of the more complicated deployments.

See the above diagram in this post for my setup explained; hopefully!

So when the WVD Management Web App Tool deploys I have to put it in my subscriptions linked to the ***jackjacktraceyco.onmicrosoft.com*** Azure AD Tenant.

However all of my WVD authentication is sourced from my ***jtlab.cloud*** Azure AD Tenant as I use Azure AD Connect to sync my users & groups from the ***LAB-DC-01*** Active Directory Domain Controller.

Once I figured out Issue #1 and the WVD Management Tool was successfully deployed I tried to login to it with my WVD Tenant Admin User (in the ***jtlab.cloud*** tenant) and got the below error:

![WVD AAD Error](/img/wvd-aad-error.png)

As you can, sort of, see it is saying that the Application with the ID of XXXXXXXX cannot be found in the directory ***jtlab.cloud*** (you’ll have to trust me on this one).

Now this isn’t really an issue caused by an error in the ARM template provided by Microsoft, its actually done exactly as I’ve told it too. The issue is my setup!

However this setup is likely to be fairly common, especially in a partner world or a provider doing a DaaS (Desktop-as-a-Service) offering.

### The Fix

Luckily this is quite an easy one.
1. Login to the Azure Active Directory Management Portal as a user with the Global Administrator role into the Azure AD Tenant where your Subscriptions and therefore WVD Management Tool is deployed – [https://aad.portal.azure.com](https://aad.portal.azure.com/)
2. Select the 'Azure Active Directory' blade > then 'App Registrations' > then search for the Application ID referenced in the error or search for 'wvd'.
   ![WVD Management App Registration](/img/wvd-mgmt-app-reg.png)
3. Select the App Registration > then select ‘Authentication’ > then scroll to the bottom of the show blade on the right until you get to the 'Supported account types' section. Change the radio button select to the option called: ***Accounts in any organizational directory (Any Azure AD directory – Multitenant)***
   ![WVD Management App All Tenants](/img/wvd-mgmt-app-all-tenants.png)
4. Click Save

And that is all you need to do to fix the login. This now allows other Azure AD tenants to sign into the WVD Management Tool that you created. Which is exactly what I need for my setup!

Hope this helps some of you out there! 