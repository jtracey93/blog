---
title: "Azure Geos, Regions Pairs, Availability Zones & Availability Sets Relationship"
summary: Demystifying all of these key Azure platform components & concepts!
date: 2019-04-09
draft: false
categories: ["Platform"]
tags: ["Azure", "Availability Set", "Availability Zone", "Best Practice", "Region", "Geography", "Gotchas", "Platform"]
thumbnail: "/img/thumbs/globe-thumb.png"
url: "/platform/azure-geos-regions-pairs-availability-zones-availability-sets-relationship"
---

A common topic I find myself explaining to customers and colleagues who haven’t worked with Azure, or any public cloud platform for that matter, is the relationship between the following Azure components:

- Geographies
- Regions
- Region Pairs
- Availability Zones
- Availability Sets
- This is quite hard to visualize when trying to explain verbally and I have drawn the same diagram on whiteboards a thousand times and it seems to make that “light bulb moment” occur to who I’m - presenting too .

So I finally decided to create them in [Lucidchart](https://www.lucidchart.com/) and share them with you all!
*(On a side not if you aren’t using [Lucidchart](https://www.lucidchart.com/) already, seriously give it a go, it’s made my diagramming a lot fast and less stressful when trying to draw connection lines in Visio)*

## The Diagrams

![Azure Platform Diagram 1](/img/az-platform-single-region.png)
The above diagram depicts that Availability Sets live within Availability Zone and they live within Regions and they finally reside within a Geography.

![Azure Platform Diagram 2](/img/az-platform-dual-region.png)
The above diagram depicts the same as the previous diagram but shows the concept of Region Pairs.

## Some Important Info…

### Availability Zones

At the time of writing this, Availability Zones aren’t available in all Regions; however they have just been announced in UK South!

[Check the latest supported Regions here.](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview#regions-that-support-availability-zones)

Also not all resources support Availability Zones, again [check the latest supported list here.](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview#services-that-support-availability-zones)

### Region Pairs

You cannot chose which region is paired with the region you chose to use, this is decided by Microsoft at the time when they build a new Region. As this is to support Geo Replicated Services (GRS) etc…

Brazil South doesn’t have another Region within the same Geography, so it is paired with South Central US. But South Central US isn’t paired back with Brazil South.

Again the latest information on Regions and where they are paired to [can be found here.](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions#what-are-paired-regions)

## Summary

Hopefully this will help you all and feel free to use the diagrams on the page (they have a transparent background, you’re welcome).

Until next time!