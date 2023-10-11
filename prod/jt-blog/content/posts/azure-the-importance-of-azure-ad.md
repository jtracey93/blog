---
title: "Azure The Importance Of Azure AD"
summary: Ever wondered how important Azure AD is in terms of Azure?! Then this is the blog post for you!
date: 2018-11-18
draft: false
categories: ["Azure AD"]
tags: ["Azure", "Azure AD", "Subscriptions"]
thumbnail: "/img/thumbs/aad-logo-thumb.svg"
url: "/azure-ad/azure-the-importance-of-azure-ad"
---

Recently the amount of conversations I have had with customers and colleagues regarding Azure AD and how it links to subscriptions etc... has been astounding.

The most common misconception I see/hear is that because it has the "AD" in its title that it is believed to be a completely separate product unrelated to the fundamental required components to get started in Azure successfully.

Also I get the question from a lot of customers who are already using Azure of "Why do we have to use separate accounts to login to Azure? "

As soon as I hear the above, I instantly smile inside, because I know they haven't understood Azure AD and its importance at the setup stage of any Azure, Office 365, Dynamics subscription creation.

## So what is the importance of Azure AD, I hear you ask?

I'll start off by showing the below diagram as this may help initiate that lightbulb moment for some of you.

![Azure AD Overview Diagram](/img/aad-ovw-diag.png)

The above diagram shows a typical setup that I have configured many times in the past. At the top we have an existing on-premise Active Directory Forest & Domain (acmecoporation.local) with all existing user, groups & computer objects in an OU structure.

In the middle we have the Azure AD Tenant with the *.onmicrosoft.com domain name of acmecorporation.onmicrosoft.com. 

**Some important things of note about the Azure AD tenant:**
- The *.onmicrosoft.com domain name must be globally unique
  - This means checking the availability of the desired domain name is vital before beginning the creation, here is a link to a great tool to do just that (Believe me when I say I have seen a - surprising amount of *.onmicrosoft.com domain names already been taken for some of my customers)
  - In some circumstances Microsoft themselves can help in tracking down a desired *.onmicrosoft.com domain name and asking the existing tenant owners if they are willing to give it to you
    - However, in most cases the existing tenant owners will not want to give this up as it will involve a large migration from one tenant to another
- The *.onmicrosoft.com domain name cannot be changed once created
- The Azure AD tenant should be treated as a single forest & domain configuration when translating to an on-premises AD deployment.
- Azure AD doesn't have any trusts between Azure AD tenants like you can do with an on-premises AD deployment
  - There is a feature called Azure AD B2B which is very similar. (A blog post for the future in its own right, watch this space)

The notes above are not a definitive list however they are the most common things I come across on a daily basis.

Users, Groups & Computers can be synchronised from on-premises Active Directory domains via the use of Azure AD Connect (formerly known as DirSync).

Azure AD Connect is an awesome tool that not only synchronises objects from on-premises to your Azure AD tenant, but it also can configure authentication methods (Password Hash Sync, Pass-Through Authentication, ADFS to name a few), all from a GUI based application.

It can also synchronise objects from several other on-premises AD topologies, however Microsofts has an article for this which is very well constructed so I will link to that here.

And finally at the bottom of the diagram are the various subscriptions that are linked to the Azure AD tenant.

Subscriptions can only be linked to one Azure AD tenant, so those subscriptions will only be able to use the users, groups & computer objects that are in the Azure AD tenant.

So I appreciate there is a lot to take in there but this just reiterates my point that it is some important to get it right at the beginning of your cloud journey.

## So how do I make the right choice when setting up an Azure Subscription?

Again this is not as easy of a task that a lot of people believe it is. So again to help ensure you get this right I have created a workflow that will help guide you to the correct choice.
![Azure AD Decision Workflow](/img/aad-decision-workflow.png)

## Summary

Again I apologise for the information overload in this article however, by taking the time to read through this article (probably several times) it should help you make the right decision around what to do.

Also it should help you avoid the nasty situation of having to migrate to a new subscription which is linked to the correct Azure AD tenant. Believe me these migrations are never fun!