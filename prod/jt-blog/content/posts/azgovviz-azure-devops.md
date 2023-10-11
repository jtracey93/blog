---
title: "AzGovViz With Azure DevOps"
summary: "I walk you through how to setup AzGovViz in Azure DevOps to automatically gather all your Azure governance data on a schedule!"
date: 2020-12-07
draft: false
categories: ["Governance"]
tags: ["Azure", "How To", "AzGovViz", "DevOps", "Pipelines", "CI/CD", "Azure DevOps", "Enterprise Scale"]
thumbnail: "/img/thumbs/azgovviz-thumb.png"

twitter:
  card: "summary"
  site: "@Jack_Ref"
  title: "AzGovViz With Azure DevOps"
  description: "I walk you through how to setup AzGovViz in Azure DevOps to automatically gather all your Azure governance data on a schedule!"
  image: "https://jacktracey.co.uk/img/thumbs/azgovviz-thumb.png"
---
## Introduction

{{< rawhtml >}}

<img src="/img/azgovviz-logo.png" align="right">

{{< /rawhtml >}}

Following on from spending a lot of time with my customers focussing on Enterprise Scale and helping them get started with Terraform & Azure DevOps. A really good question was raised around documentation of the Azure Governance estate that either already exists today or that we are deploying via Terraform.

Luckily there is already a brilliant, free tool out there for you that can help with this exact task called AzGovViz. AzGovViz is effectively just a PowerShell script (although it's over 10,000 lines of PowerShell!) that is written and looked after by Julian Hayward, a Customer Engineer at Microsoft based in Germany!

In AzGovViz's own words: *"AzGovViz is intended to help you to get a holistic overview on your technical Azure Governance implementation by connecting the dots."*

Checkout the links below for more information:

- [AzGovViz GitHub Repo](https://github.com/JulianHayward/Azure-MG-Sub-Governance-Reporting)
- [Azure CAF Governance Tools](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/reference/tools-templates#govern)

## So What Is This Blog Post About?

Good question. In this blog post I will walk you through how to setup AzGovViz with Azure DevOps! 

It really is as simple as it looks, and requires minimal permissions which keeps security teams very happy indeed!

So lets get into it!

## Video Walk Through

{{< youtube TEtnkebJzsc >}}
