---
title: "Azure Subscription Migrations"
summary: Ever been tasked with working out how to migrate resources between subscriptions? This is the blog post for you!
date: 2021-12-17
draft: false
categories: ["Migration"]
tags: ["Azure", "Azure AD", "Migration", "Subscriptions", "How To", "EA To CSP", "MCA", "Azure Plan", "CSP"]
thumbnail: "/img/thumbs/migration-thumb.png"
url: "/migration/azure-subscription-migrations"
twitter:
  card: "summary"
  site: "@Jack_Ref"
  title: "Azure Subscription Migrations"
  description: "Ever been tasked with working out how to migrate resources between subscriptions? This is the blog post for you!"
  image: "https://jacktracey.co.uk/blog/img/thumbs/migration-thumb.png"
---
# Version History
## December 2021 - Update

> I have been working with the internal Microsoft docs and engineering teams to get what I have created here as a set of official Microsoft Docs. These are now live:
> - [Azure subscription and reservation transfer hub](https://docs.microsoft.com/azure/cost-management-billing/manage/subscription-transfer)
> - [Transfer an Azure subscription to an Enterprise Agreement (EA)](https://docs.microsoft.com/azure/cost-management-billing/manage/mosp-ea-transfer)
> 
> Please treat these as a source of truth going forward. However, I will try to keep this page up-to-date also!

# Introduction

Recently I have had an abundance of requests from our regarding Azure Subscription migrations. Whether it be from PAYG (Pay-As-You-Go) to CSP, EA to CSP, CSP to PAYG, EA to MCA, MCA to CSP or just PAYG to PAYG.

Whatever the source and destination subscription model is, the answer I give is the same!

Every migration for each customer is going to be different 99% of the time and in the majority of cases is not as simple as the click of the migrate button from within the portal and away you go. Perhaps one day it will be; I'll be a very happy man that day for sure. And there have been some great improvements to this process in the EA to CSP space recently that make it almost seamless!

So in this post I will share with you how I approach these requests and a tool I have developed to help speed the assessment process up significantly.

***Please note this article will focus on subscription level migrations, however the tool accommodates for Resource Group level migrations as well!***

# Before you even think about migrating...

There are a few key points of information that you need to gather/understand when starting with one of these requests.

1. Why does the customer want to migrate subscriptions?
2. What subscription model are the source and destination subscriptions using; or going to use?
   - PAYG
   - CSP V1
   - CSP V2 - aka Azure Plan
   - EA
   - MCA
   - Other... (MSDN, BizSpark, EOPEN, Azure Pass etc...)
3. An export of all resources from all of the source subscriptions.
4. Timescales for migration completion.

All of these questions are important to have an answer for before beginning your approach to the migration.

Questions 1 and 4 are more to help understand the "why" from the customer and to set expectations early on timescales. Because we all know sometimes timescale expectations can be unrealistic and it's important for us to reset them accordingly if we know the migration is likely to take longer than the time available. 

Questions 2 and 3 will help define some technical paths you will need to follow and various limitations that each combination may have.

# Subscription Migration Support Matrix

I feel now is a good time to lay out all of the combinations for subscription migrations and what initial approach should be taken.

***Apologies for the length of this table but there are a lot of possible different combinations!***

Source Subscription Model | Destination Subscription Model | Migration Supported | Migration Approach Notes
:---: | :---: | :---: | :---:
PAYG | EA | Yes | Just a back-end Azure billing change. No downtime
PAYG | CSP V1 | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. Check services are available in CSP. No classic (ASM) resources are supported in CSP V1.
PAYG | CSP V2 (aka Azure Plan) | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. Check services are available in CSP.
PAYG | MCA | Yes | Just a back-end Azure billing change. No downtime. See: [https://docs.microsoft.com/azure/cost-management-billing/manage/billing-subscription-transfer](https://docs.microsoft.com/azure/cost-management-billing/manage/billing-subscription-transfer) 
PAYG | MSDN/BizSpark | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. 
PAYG | PAYG | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. 
EA | PAYG | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. 
EA | CSP V1 | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. Check services are available in CSP. No classic (ASM) resources are supported in CSP V1.
EA | CSP V2 (aka Azure Plan) | Yes | Just a back-end Azure billing change. Some limitations with reservations & some marketplace items (must be available in CSP marketplace to transfer). No downtime. Only available to CSP Direct Bill Partners who are Azure Expert MSPs. See: [https://docs.microsoft.com/azure/cost-management-billing/manage/mpa-request-ownership](https://docs.microsoft.com/azure/cost-management-billing/manage/mpa-request-ownership)
EA | MCA | Yes | Just a back-end Azure billing change. No downtime. See: [https://docs.microsoft.com/azure/cost-management-billing/manage/mca-setup-account](https://docs.microsoft.com/azure/cost-management-billing/manage/mca-setup-account)
EA | MSDN/BizSpark | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations. 
EA | EA | Yes/No/Not Common | If different Azure AD Tenant same as EA to PAYG. If same Azure AD Tenant, why are you migrating as you can just change subscription owner instead. N.B. this not a migration I have ever come across to date.
MSDN/BizSpark | EA | Yes | Just a back-end Azure billing change. No downtime
MSDN/BizSpark | PAYG | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations.
MSDN/BizSpark | CSP V1 | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations.
MSDN/BizSpark | CSP V2 (aka Azure Plan) | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations.
MSDN/BizSpark | MSDN/BizSpark | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations.
MSDN | MCA | Yes | Just a back-end Azure billing change. No downtime. See: [https://docs.microsoft.com/azure/cost-management-billing/manage/billing-subscription-transfer](https://docs.microsoft.com/azure/cost-management-billing/manage/billing-subscription-transfer)
BizSpark etc. | MCA| Yes| Resources must be migrated between subscriptions. Possible downtime & limitations. Not all offer types supported to MCA, see: [https://docs.microsoft.com/azure/cost-management-billing/manage/billing-subscription-transfer#supported-subscription-types](https://docs.microsoft.com/azure/cost-management-billing/manage/billing-subscription-transfer#supported-subscription-types)
CSP V1 | MSDN/BizSpark | Yes/Not Common | Resources must be migrated between subscriptions. Possible downtime & limitations.
CSP V2 (aka Azure Plan) | MSDN/BizSpark | Yes/Not Common | Resources must be migrated between subscriptions. Possible downtime & limitations.
CSP V1 | EA | Yes/No/Not Common | Believe this would have to be treated as if it were PAYG to PAYG as CSP subscription has some back-end billing differences. Therefore doubtful that EA subscription import/billing change process will not work. Resources must be migrated between subscriptions. Possible downtime & limitations.
CSP V2 (aka Azure Plan) | EA | Yes/No/Not Common | Believe this would have to be treated as if it were PAYG to PAYG as CSP subscription has some back-end billing differences. Therefore doubtful that EA subscription import/billing change process will not work. Resources must be migrated between subscriptions. Possible downtime & limitations.
CSP V1 | PAYG | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations.
CSP V2 (aka Azure Plan) | PAYG | Yes | Resources must be migrated between subscriptions. Possible downtime & limitations.
CSP V1 | CSP V1 | Yes | Back end billing change but must be requested in certain way and currently no automated way to do this. See: [https://docs.microsoft.com/azure/cloud-solution-provider/customer-management/switch-subscription-to-different-csp-partner](https://docs.microsoft.com/partner-center/switch-azure-subscriptions-to-a-different-partner)
CSP V2 (aka Azure Plan) | CSP V2 (aka Azure Plan) | Yes | Process & tooling now available in preview to partners. See: [https://docs.microsoft.com/azure/cost-management-billing/manage/azure-plan-subscription-transfer-partners](https://docs.microsoft.com/azure/cost-management-billing/manage/azure-plan-subscription-transfer-partners)
CSP V1 | CSP V2 (aka Azure Plan) | Yes | Can only be done be the CSP partner who provides the Azure Subscriptions to you. Also cannot be done when transferring between CSP partners as above; change CSP partner first then get new partner to complete this migration for your subscriptions. See: [https://docs.microsoft.com/partner-center/azure-plan-transition](https://docs.microsoft.com/partner-center/azure-plan-transition)

# EA To CSP V2 (aka Azure Plan) Notes

From working at multiple CSP partners during my career so far I have lived in the CSP program for some time now. During my time at my last partner we were a CSP Direct Tier 1 Partner and also had access to the tooling to be able to migrate customers from EA based subscription to our CSP platform as per the documentation [here](https://docs.microsoft.com/en-gb/azure/cost-management-billing/manage/mpa-request-ownership).

I was part of the team that helped build our internal processes for EA to CSP V2 migrations and also completed a number of them. So below I will list a few of my tips and notes of things I believe are important to do at each phase of an EA to CSP V2 migration.

The most important thing to remember is once migrated to CSP there is no easy way to go back! Planning is essential!

### Migration Assessment/Qualification

- Does the customer have any Azure Reservations in place for any resources (VMs, Storage Accounts, Data Lakes, etc.)?
   - As per [docs](https://docs.microsoft.com/en-gb/azure/cost-management-billing/manage/mpa-request-ownership#azure-reservations-transfer), these aren't transferred during the migration. They either need to be cancelled and re-purchased once migrated to CSP V2 or you can leave them in place as they are and then just purchase them on CSP when they expire to renew them.
      - *I never got the chance to test the latter option out but I know other who have and say it works as the Subscription IDs do not change*
   - May be a good idea to raise a CSP support ticket if you want some additional facts on this topic to help you make a decision on what to do.
- Does the customer have any subscriptions that they want to migrate that utilise the Dev/Test offer?
   - If so, these can still be migrated but the discounted Dev/Test pricing will not be kept when migrated to CSP and the resources in these subscriptions will be charged at the normal CSP price list (effectively converting them to normal CSP V2 subscriptions or PAYG pricing; this is the way I used to explain it to customers).
- Classic Azure Resources **are supported** in this type of migration
- Not all Marketplace products can be transferred
   - If the Marketplace product does not exist in the CSP marketplace, then the subscription cannot be migrated from EA To CSP V2
- Existing Azure Support plans/packages cannot be used once subscriptions are migrated to CSP
   - Only the CSP can raise support tickets for the subscriptions once migrated.
   - Ensure these existing support plans/packages are cancelled if no longer required.
- Any remaining Azure EA Commit funds do not transfer across to CSP, it will be lost if there is any value remaining once migrated.
   - My advice is to plan for the migration to take place at a time when the EA commit has been fully used/ran out to ensure the customer doesn't lose any funds.
- If the customer spends a large amount each month in Azure, it is likely their migration to CSP may not automatically be permitted. You may need to engage with your Microsoft PDM (Partner Development Manager) & the customers Microsoft Account Executive to ensure the migration to CSP is the best thing for the customer.
   - There is a formal process to go through with Microsoft and this can take a few weeks to go through
   - My best advice is to engage with Microsoft as soon as possible if you think your customer may fall into this scenario.

### Performing the Migration 

- The migration process is a **no downtime** migration, it is just a billing change on the backend. 
   - *Trust me I have done this whilst sitting next to a customer, worked perfectly!*
- You can only migrate subscriptions between the same Azure AD Tenant during this process.
   - This means if the customer has subscriptions linked to different Azure AD Tenants on their EA today, you will need to setup a separate CSP Partner Relationship for each of the Azure AD Tenants (*.onmicrosoft.com domains).
- Customer must accept new MCA (Microsoft Customer Agreement) and this be updated on each of their CSP customer records.
- The whole subscription migrates as a whole, not individual resources etc.
   - The subscription ID does not change
   - This means that if one resources doesn't support the migration then the whole subscription cannot be migrated to CSP V2.
      - If you come across this scenario with just one resources that wont migrate, you can always try and migrate the troublesome resource to another subscription to enable you to migrate the subscription to CSP V2.
- **You must** create an Azure Plan for the customer via the CSP portal before you can perform a migration from EA to CSP V2 for any customer subscriptions! 
   - See: [https://docs.microsoft.com/en-us/partner-center/purchase-azure-plan](https://docs.microsoft.com/en-us/partner-center/purchase-azure-plan)
- **Required User Permissions:**
   - **CSP:**
      - Global Admin or Admin Agent in Partner Center
   - **Customer:**
      - Global Admin on each of the Azure AD Tenants
      - Owner of each Azure Subscriptions
      - Enterprise Account Owner where the subscriptions are held
- The user that the customer logs in as to complete their side of the migrations from must be an account that is held within the same/native Azure AD Tenant, it **cannot** be a guest account invited to the tenant.
- All RBAC permissions are kept in place, nothing changes during the migration process!

### Post Migration

- Once migrated you will notice that from the CSP administration side you cannot gain DAP (Delegated Admin Access) to the customer subscriptions even though you can see them from a billing perspective in the CSP portal.
   - This is by design and therefore you have to manually find the ID of the CSP Foreign Principal and then apply this as Owner (RBAC Role) to each of the subscriptions.
      - This can only be done by a user is the native Azure AD Tenant with Owner (RBAC Role) permissions. *I usually do a screen share with the customer who I am doing the migration with to complete this step with them by either talking them through it or taking control myself!*
   - You also need to do this as a CSP to get any PEC (Partner Earned Credit) back on the customers ACR (Azure Consumed Revenue)!

#### CSP Foreign Principal PowerShell Script

You can use the below PowerShell script to help apply the CSP Foreign Principal permissions:

```go {linenos=false}
## Connect to Customer AAD Tenant - Change 'xxxx' to the Azure AD Tenant ID which the subscriptions are linked to:
Connect-AzAccount -TenantId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## Get CSP Foreign Principal ID - Change 'PARTNER NAME' to whatever your CSP Partner Name is:
Get-AZRoleAssignment | where DisplayName -like "Foreign Principal for 'PARTNER NAME'*" | fl DisplayName, ObjectID

## Example Command Output:
DisplayName : Foreign Principal for 'CSP PARTNER NAME' in Role 'TenantAdmins' (CUSTOMER NAME)
ObjectId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## Set CSP Foreign Principal With Permissions Over Required Subscriptions - Insert correct ObjectID of CSP Foreign Principal & Customer Subscription ID:
New-AZRoleAssignment -ObjectId '{ObjectId from previous step}' -RoleDefinitionName Owner -Scope /subscriptions/XXXXX
```
> Repeat the above steps for all Azure Subscriptions that you have migrated!

> The ObjectID is not the same across Azure AD Tenants, it's different in each tenant!

# Assessing Resource Migrations Between Subscriptions

As you have seen in the table above, the majority of migrations require you to migrate the actual Azure resources between subscriptions. As mentioned before and in the table rows, this sometime incurs downtime and also there are various limitations per Azure resource type (VM’s, NSG’s, App Services etc…).

Now there used to be a handy little tool that someone created for CSP V1 migration assessments called the “Azure CSP Assessment”. This was an Azure hosted web app located here: [https://azurecspassessment.azurewebsites.net/](https://azurecspassessment.azurewebsites.net/) but as you can see the site is now longer up and running 🙁

However using the tool was always a risk as the list of resources that support subscription migration and the various limitations changes at quite a pace; as does everything in the Azure world, right!

So it used to mean that I get an export of the customers source Azure subscription resources and resource types using the below PowerShell command:

```go {linenos=false}
Get-AzResource | Export-Csv PATHTOFILE.csv
```

Then using the exported CSV file I would use Excel and the following below pages on Azure Docs to go through each resource type and check its compatibility and limitations:

- [https://docs.microsoft.com/en-gb/azure/azure-resource-manager/move-support-resources](https://docs.microsoft.com/en-gb/azure/azure-resource-manager/move-support-resources)
- [https://docs.microsoft.com/en-gb/azure/azure-resource-manager/resource-group-move-resources](https://docs.microsoft.com/en-gb/azure/azure-resource-manager/resource-group-move-resources)
- [https://docs.microsoft.com/en-us/azure/cloud-solution-provider/overview/azure-csp-available-services](https://docs.microsoft.com/en-us/azure/cloud-solution-provider/overview/azure-csp-available-services) – Only when migrating to CSP

To say this was long winded and painful is an understatement, trust me!

# Azure Resource Migration Support Tool

So that’s why I have created a handy Excel Workbook that does all the work in comparing against the information in links 1 and 2 above with a simple copy and paste of specific columns from the exported CSV.

I also thought it would be a shame not to share this tool so here it is available for any of you reading this to use for free!

- [Azure Resource Migration Support Tool](https://jacktracey.co.uk/AzureResourceMigrationSupportV9.xlsx)

All instructions on how to use the tool are on the “Intro Page” sheet within the workbook/spreadsheet.

I will be periodically checking the Azure Docs pages and updating any changes to resources that are now supported for migration to this tool and I will update this page with the latest version of the tool.

> Thanks to Martin Baker for contributing some changes and updates to the tool! #AzureFamily

# What do I do once I’ve used the tool to assess my resources…

Well firstly, please comment below or get in touch with me via Twitter, LinkedIn or e-mail me with any feedback or features you would like in newer releases of the tool.

Once you’ve done that and used the tool to assess your resources in your source subscription, it is highly likely you have a good idea about how you need to proceed.

I strongly suggest running this as a project within your company as it is not as simple as clicking a migrate button. I’ve even called it a “Virtual Data Centre Move” as it really can have the same potential devastating unplanned outages if you don’t treat it with the correct attention and detailed planning.

Personally I suggest building a project plan, if you have a Project Manager to help you, even better. Detail every task you are going to need to do before, during and after the migration, some examples below:

- Create destination subscription
- Attach destination subscription to existing (same as source subscription) Azure AD tenant – **THIS IS MANDATORY AT THIS TIME, BOTH SUBSCRIPTIONS MUST BE IN THE SAME AZURE AD TENANT**
- Change Public IP SKUs for Resources: X, Y & Z
- etc…
Once you have your plan built, start raising RFC/Changes (if required) to get this work completed. Some of this work may even require re-provisioning resources to get them on the correct SKUs etc… so it would also be prudent to get any other internal teams involved to assist with testing etc… if you aren’t able to do this yourself during your changes.

Nobody likes the dreaded out of hours phone call when something you couldn’t test doesn’t work after a change.

Once you’ve made all of the prerequisite changes, its now time to probably download the latest version of the tool, export all your resource into a CSV again and check for any additional changes that you may need to make as things may of changed from the Azure side.

If nothing has then that’s great news as you haven’t got to go back through the whole process again. You should now make sure that all Resource Providers in use in the source subscription are registered in the destination subscription.

To check the Resource Providers in use in the either subscription use the following PowerShell command (please note you’ll need to change subscriptions within your PowerShell session using the first 2 commands in the below block):
```go {linenos=false}
## Find Subscription ID ##
Get-AzSubscription

## Change Subscription Within PowerShell Session ##
Select-AzSubscription -SubscriptionId 'PASTE ID HERE'

## Check Resource Providers For Selected Subscription ##
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

You should get the below output for the Resource Provider command. (I’m using CloudShell, check it out if you aren’t already):

![PowerShell Show Resource Providers State](/img/get-az-providers.png)

Compare both subscription outputs against each other, using Export-Csv may be your friend here. And then register any providers in the destination subscription that are registered in the source subscription but not in the destination.

To register Resource Providers use the below command (again please note you’ll need to make sure you’ve changed your sessions selection to the correct subscription again using the above commands):
```go {linenos=false}
## Register Resource Provider ##
Register-AzResourceProvider -ProviderNamespace 'PROVIDER NAMESPACE PLEASE CHANGE'
```

You should get the below output when registering a provider:

![PowerShell Register Resource Provider](/img/register-rp-pwsh.png)

Once you have registered all the required providers run one last comparison check and then you can proceed to actually pushing that ‘Move to another subscription’ button on your resources/resource groups as per your plan.

![Portal Migrate Resource](/img/az-migrate-resource-portal.png)

# Summary
As you can see by the length of this article the process is not always straight forward and can be quite a long process from start to finish.

Please let me know your feedback for the tool via any method that I mentioned above.

And more importantly I hope this article helps you plan your migration to be successful.
