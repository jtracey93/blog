---
title: "Azure Spring Clean 2020 - Azure Resource Graph"
summary: Here is my supporting blog post entry for the Azure Spring Clean 2020 on the Azure Resource Graph!
date: 2020-02-17
draft: false
categories: ["Community"]
tags: ["Azure Spring Clean", "Azure", "Governance", "Kusto", "Partners", "How To", "Dashboards"]
thumbnail: "/img/thumbs/spring-clean-day-11-thumb.jpg"
twitter:
  card: "summary"
  site: "@Jack_Ref"
  title: "Azure Spring Clean 2020 - Azure Resource Graph"
  description: "Here is my supporting blog post entry for the Azure Spring Clean 2020 on the Azure Resource Graph!"
  image: "https://jacktracey.co.uk/blog/img/thumbs/spring-clean-day-11-thumb.jpg"
---

![Spring Clean Day 11](/img/spring-clean-day-11.jpg)

## A Quick Thank You

Firstly, before we get started I want to say a huge thanks to [Joe Carlyle](https://wedoazure.ie/) and [Thomas Thornton](https://thomasthornton.cloud/) for arranging and organising this event. 

We all know these events take a huge amount of effort in planning and making them become a reality, so congratulations off to your both!

Yet again, another great example of the awesome [#AzureFamily](https://twitter.com/hashtag/AzureFamily?src=hashtag_click) in action! Get Involved!

## Azure Resource Graph Overview

The Azure Resource Graph is a service provided by Azure, based on the Kusto Query Language (KQL), that allow you to query quickly and efficiently across one or many subscriptions to explore resources and their properties within your Azure environment. 

The Azure Portal’s search bar and the ‘All Resources’ blade are all powered by Azure Resource Graph as well as the new Change History feature that has been released in the past few months. 

So it is likely you have been already utilising it without knowing so!

In my opinion it’s the best tool for digging into and discovering what resources, or properties of a set of resources, are in your Azure Subscriptions at speed! 

It’s purpose built for this task so it makes it much more attractive to use over PowerShell or AZ CLI in my opinion.

> It also supports Azure Delegated Resource Management (Azure Lighthouse), so for MSPs/Partners this is a great benefit to query all your customers at scale and speed!

## How Does It Work Behind The Scenes?

Azure Resource Graph works by keeping a cached copy of your resources and their properties within a few Log Analytics tables; however you don't have to deploy a Log Analytics Workspace and configure this, its all automatic and more importantly, free!

These tables get updated by the Azure Resource Manager every time a resource is created, updated or deleted by yourself or others. It also regularly performs full scans of your resources to ensure nothing is missed.

> Microsoft aim to get 95% of resource changes into Resource Graph within 1 minute of the change completion

> The frequency of the full scans is not currently known or documented

## Kusto Query Language - KQL

Kusto Query Language (KQL) is the language that the Azure Resource Graph query language is based upon. KQL is very, very similar to T-SQL, so for any SQL DBAs out there, you should find this pretty easy to get off the ground with also.

If you are used to using Azure Monitor/Log Analytics to search through logs and create alerts, then you'll likely be just fine with using Azure Resource Graph.

However, if this is your first time using KQL then below are links to a few resources to help you learn the basics of the query language:

- KQL Overview Docs - [https://docs.microsoft.com/en-us/azure/kusto/query/](https://docs.microsoft.com/en-us/azure/kusto/query/)
- KQL Tutorial - [https://docs.microsoft.com/en-us/azure/kusto/query/tutorial?pivots=azuredataexplorer](https://docs.microsoft.com/en-us/azure/kusto/query/tutorial?pivots=azuredataexplorer)
- Azure Resource Graph Query Language Docs - [https://docs.microsoft.com/en-us/azure/governance/resource-graph/concepts/query-language](https://docs.microsoft.com/en-us/azure/governance/resource-graph/concepts/query-language)
- FREE Pluarlsight Course - [https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)
- FREE demo Log Analytics Workspace with sample data to run KQL queries against - [https://portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade)

## Resource Graph Explorer & Queries

These are 2 new blades/sections to the Azure Portal to help you both construct, store and share your Azure Resource Graph queries that you create. 

### Azure Resource Graph Explorer {{< rawhtml >}} <img src="/img/resource-graph-explorer.png"> {{< /rawhtml >}}

Azure Resource Graph Explorer has been great for me personally, as I found using Azure Resource Graph via PowerShell or AZ CLI a little clunky in the past to just look for information about resources etc...

With the Azure Resource Graph Explorer you can easily construct and build your own queries with a intuitive and graphical interface. You also get a small amount of help on syntax from IntelliSense/Code. You can then instantly run the queries you create and see their outputs. 

It’s a great place to get your query built, tested and working. You can then save them from their for future use or pin them to a dashboard; more on this later!

![Azure Resource Graph Explorer Screenshot](/img/arg-exp-1.png)

### Azure Resource Graph Queries {{< rawhtml >}} <img src="/img/resource-graph-queries.png"> {{< /rawhtml >}}

Azure Resource Graph Queries is another blade in the portal where all of your saved queries are stored. You can even share them with other users if you wish too.

![Azure Resource Graph Queries Screenshot](/img/arg-qry-1.png)

## Getting Started - Running Your First Query

Microsoft have helpfully documented a lot of starter & some advanced queries to help you get started.

- Starter Queries - [https://docs.microsoft.com/en-us/azure/governance/resource-graph/samples/starter?tabs=azure-cli](https://docs.microsoft.com/en-us/azure/governance/resource-graph/samples/starter?tabs=azure-cli)
- Advanced Queries - [https://docs.microsoft.com/en-us/azure/governance/resource-graph/samples/advanced?tabs=azure-cli](https://docs.microsoft.com/en-us/azure/governance/resource-graph/samples/advanced?tabs=azure-cli) 

So get logged into the Azure Portal, open the Azure Resource Graph Explorer blade [or click here](https://portal.azure.com/#blade/HubsExtension/ArgQueryBlade) and open the 'Starter Queries' docs page and lets walk through running your first query!

1. Okay so you should currently be at this screen in the Azure Portal
   ![ARG Step 1](/img/arg-step-1.png)
2. From the 'Starter Queries' docs page copy the first example query
   ![ARG Step 2](/img/arg-step-2.png)
3. Paste it into the Resource Graph Explorer Query page
   ![ARG Step 3](/img/arg-step-3.png)
4. Select the query, it will get highlighted in blue, then press 'Run Query' and look at the results
   ![ARG Step 4](/img/arg-step-4.png)

**Easy, right?!**

Let's make it a little more powerful and pretty, with a few additions to the query:

5. Add "by type" to the end of the query on line 2, the complete query would be:
    ```go {linenos=false}
    Resources
    | summarize count() by type
    ```
    ![ARG Step 5](/img/arg-step-5.png)
6. Press 'Run Query' again and look at the new results that have been output
   ![ARG Step 6](/img/arg-step-6.png)
   > We are now seeing the count of all resources that I have access to see, grouped by the Resource Type property

**Okay that's powerful, but not very pretty**

7. Select the 'Charts' tab in the results output section and select a chart type. I'll select a 'Bar chart' for this example:
   ![ARG Step 7](/img/arg-step-7.png)
8. Sometimes, if ARG knows the requested output may cause some performance issues displaying it, you will be warned about it. You can hit 'Okay' just be aware sometimes the results can behave in a lagging fashion.
   ![ARG Step 8](/img/arg-step-8.png)
9. Look at the amazing graphical output you now have, visualising your data!
   ![ARG Step 9](/img/arg-step-9.png)
10. If you want to sort them in order of most used, just add a new line to the query with " | sort by count_ ", the complete query would be:
    ```go {linenos=false}
    Resources
    | summarize count() by type
    | sort by count_
    ```
    ![ARG Step 10](/img/arg-step-10.png)
11. Re-run the query and look at the bar graph now showing the resource types in order from most to least used
    ![ARG Step 11](/img/arg-step-11.png)

## Creating a Dashboard

For the eager eyed readers of this post, you have probably seen the 'Pin to dashboard' option in some of the screenshots. 

Yes it is that easy to add them the the last viewed dashboard you have, how useful and powerful is that?!

Very helpfully, the [Exchange Goddess (Phoummala Schmitt)](https://twitter.com/exchangegoddess), has shared a pre-created dashboard with the ARG queries all built-in for you all to import and try out. Here is what it looks like on my environment:
![Exchange Goddess ARG Dashboard](/img/arg-egod-dashboard.png)

Pretty cool if you ask me!

### Import The Dashboard For Your Envrionment

1. Download the dashboard JSON file from my [GitHub Repo](https://github.com/jtracey93/PublicScripts/blob/master/Azure/Resource%20Graph/Dashboards/exchangeGoddessARGDashboard.json) 
2. Create a new Azure Dashboard
   ![ARG Dashboard Step 1](/img/arg-dash-1.png)
3. Click 'Done Customizing' at the top in purple
4. Upload the Exchange Goddess downloaded dashboard you got in step 1, by clicking 'Upload' and selecting the file where you saved it to in step 1
   ![ARG Dashboard Step 2](/img/arg-dash-2.png)
5. Done! Magical, right?!

> The dashboard will rename itself to 'Azure Inventory Dashboard' FYI

All of the ARG queries are in the JSON file, so go open it up in Visual Studio Code and take a look at the queries and amend them to make it more tailored to your requirements!

## Summary

Hopefully you now see why I think Azure Resource Graph, is much more powerful than using PowerShell or AZ CLI to query resources and their properties!

The most important thing to remember is that you are not going to delete or change any resources by just playing with Azure Resource Graph Explorer & Queries, it purely is just getting data from the Log Analytics tables it stores that data in as mentioned at the start. So don't think that you can't have a go as you don't know how to use it!

The best way to learn is by doing in my book! Go get querying!

Also I have uploaded some queries that I have created myself for some specific requirements I had that you may find useful to my [GitHub Repo. Go check them out.](https://github.com/jtracey93/PublicScripts/tree/master/Azure/Resource%20Graph/Queries)

As always, please comment below if you found this useful or you have any questions. Or reach out to me on Twitter!