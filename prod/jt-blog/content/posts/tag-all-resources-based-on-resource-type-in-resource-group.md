---
title: "Tag All Resources Based on Resource Type in Resource Group"
summary: Time to share a handy little PowerShell script to help with your tags on resources
date: 2019-01-27
draft: false
categories: ["Scripts"]
tags: ["Azure", "How To", "PowerShell", "Scripts", "CLI", "Tagging", "Tags", "Governance"]
thumbnail: "/img/thumbs/key-tags-thumb.jpg"
url: "/scripts/tag-all-resources-based-on-resource-type-in-resource-group/"
---

Recently I've been working closely with a few of our Azure Consultants in our delivery team around defining best practices and how we can speed up/automate as much of our deployment standards on a clients Azure environment.

At the moment we are focusing a lot on governance standards, these encompass features like:

- Azure Policy
- Resource Tags
- Management Groups
- etc...

Resource Tags are an essential part of any Azure environment. My take on them is the more there are the better! (As long as keys are consistent across resources of course)

## The Problem...

We do a lot of retro-tagging on resources in our clients Azure environments to assist in bringing them inline with our standards. And also to enable their Azure subscriptions to slot into our management tools, which we use tags heavily for to control certain things etc...

One problem we face quite regularly is that some resources have started to of been tagged, whilst others haven't. Normally we find the tags that have made their way onto resources is actually down to a deployment template from a marketplace resource. like Jenkins Server, rather than being manually created and set by an IT admin.

This normally means that scripting tag creation/setting on resources with most of the existing PowerShell scripts we have and are also available over the internet are unusable. This is because these scripts generally just replace the tags, if any are in place already; not ideal at all!

## The Solution...

So with the above problem becoming ever more a time consuming block for our teams internally, I decided to get my head down in VS Code and write a new PowerShell script that will overcome this issue!

I'm glad to say that after about 3/4 hours of work and constant testing with different environment scenarios, I accomplished it!

The script is available on my ['PublicScrips' GitHub repository here!](https://github.com/jtracey93/PublicScripts/blob/master/Azure/PowerShell/Tags/ApplyTagsAllResourceTypesInResourceGroupV1.0.ps1)

Please feel free to download, use, edit, alter and report any issues with the script either below in the comments or via GitHub directly and I will do my best in my spare time to resolve any issues reported.

Obviously as with any script you find on the internet, please test it on a subset of resources before letting it loose on your entire environment. Whilst I have tested this script over 50+ times and on different environments to ensure if handled all possible scenarios, I cannot guarantee that to anyone that it is 100% error free (although I don't think its bad, even if I do say so myself :D ).

## Summary

Enjoy the script! And please do let me know of any issues you find.

Also let me know of any future feature requests or other common scripting issues you face that you may like me to tackle in the future, in the comments below or via Twitter!