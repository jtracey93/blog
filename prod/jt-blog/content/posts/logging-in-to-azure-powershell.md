---
title: "Logging in to Azure Powershell"
summary: My step by step guide for logging in to Azure via PowerShell to the correct Tenant every time!
date: 2019-12-16
draft: false
categories: ["How To's"]
tags: ["Az", "Az PowerShell", "Azure", "Azure AD", "Best Practice", "CLI", "Gotchas", "How To", "How To's", "PowerShell", "Scripts", "Subscriptions"]
thumbnail: "/img/thumbs/pwsh-logo-thumb.jpg"
image: "/img/thumbs/pwsh-logo-thumb.jpg"
url: "/how-tos/logging-in-to-azure-powershell"
---

Recently I helped out a friend on Twitter with an Azure PowerShell issue they were having logging in to their subscriptions with the ‘Az’ PowerShell module.

It should also be noted that you can easily use other tools like Windows Terminal to access CloudShell or access it directly from https://shell.azure.com

However this scenario is for where PowerShell is required locally. And more importantly you need to avoid SSO (Single Sign-On) on devices from just bypassing the Azure AD login pages as you need to login as a different user or to a different tenant etc…

Once they had confirmed my proposed solution/idea fixed the issue with not being able to login to Azure via PowerShell and being able to access what they wanted, I thought it would be rude not share my useful tip with the rest of you; so here we go.

## How I Always Login To Azure PowerShell

Below I will share with you how I login to Azure via the ‘Az’ PowerShell module every time without fail to avoid access issues.

1. **Open an In-Private Browser Window** - *N.B. make sure you don’t have any other In-Private tabs logged into Azure or Office 365 open at the same time in the same browser client when doing this.*

   ![](/img/az-pwsh-login-1.png)

2. **Browse To:** https://portal.azure.com

   ![](/img/az-pwsh-login-2.png)

3. **Login To The Azure Account You Want To Access Via PowerShell**

   ![](/img/az-pwsh-login-3.png)

4. **Confirm You Are Logged In To The Correct Azure Account**

   ![](/img/az-pwsh-login-4.png)

5. **Open PowerShell**

   ![](/img/az-pwsh-login-5.png)

6. **Run the command:**
   ```go {linenos=false}
    Login-AzAccount -UseDeviceAuthentication
    ```
7. **You Will Be Given Instructions To Follow To Complete The Login**

   ![](/img/az-pwsh-login-7.png)

8. **In The In-Private Browser Tab You Have Open Browse To:** https://microsoft.com/devicelogin **OR** https://aka.ms/devicelogin

   ![](/img/az-pwsh-login-8.png)

9. **Enter The Code You Were Given In PowerShell** - *N.B. It’s covered in red on my screenshot (for security reasons)*

   ![](/img/az-pwsh-login-9.png)

10. **Confirm The Account To Login To PowerShell With** - *N.B. This is the account that is already signed in from earlier*
    
    ![](/img/az-pwsh-login-10.png)

11. **You Should Then See A Confirmation Page Stating You Are Logged Into Azure PowerShell**
    
    ![](/img/az-pwsh-login-11.png)

12. **You Can Now Run Az PowerShell Commands**
    
    ![](/img/az-pwsh-login-12.png)

## Other Useful Az PowerShell Commands

```go {linenos=false}
Select-AzSubscription -SubscriptionId ‘SUBSCRIPTION-ID-GUID’
```
*N.B. Use Get-AzSubscription to get a list of all subscriptions and their ID’s in the AAD Tenant you have logged into and have access too (as shown in step 12)*