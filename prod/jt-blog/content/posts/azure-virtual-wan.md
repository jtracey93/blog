---
title: "Azure Virtual WAN"
summary: An overview of the new "SD-WAN" style networking service in Azure, aptly named Azure Virtual WAN!
date: 2018-10-14
draft: false
categories: ["Azure Networking"]
tags: ["Azure", "BGP", "Networking", "Routing", "SD-WAN"]
thumbnail: "/img/thumbs/az-vwan-thumb.png"
url: "/azure-networking/azure-virtual-wan"
---

Back in July 2018, Microsoft announced the public preview of a 2 new products to the Azure family, Azure Virtual WAN & Azure Firewall. [Click here to read that announcement.](https://azure.microsoft.com/en-gb/blog/announcing-public-preview-of-azure-virtual-wan-and-azure-firewall/)

Following the announcement I instantly registered for the public preview of both products so I could test them in one of my Azure demo environments.

Myself and a colleague managed to allocate some time during our working week to test out Azure Firewall, however we never got a chance to test Azure Virtual WAN.

However during the time that has passed since the public preview announcement and now, Microsoft Ignite has been and gone and both products have now been released to GA! (plus I have also started a new role at an MSP in London, so a lot going on in a short space of time) [Click here to read the GA announcement (about a third of the way down)](https://azure.microsoft.com/en-us/blog/azure-networking-fall-2018-update/)

However since starting my new role I have had some time to test out Azure Virtual WAN as well.

So let's get into Azure Virtual WAN in a bit more detail from my experiences deploying them and from what I have read and watched from various resources.

## Azure Virtual WAN

So Azure Virtual WAN is a networking service provided by Microsoft that allows you to connect your various branch offices together and also connect these to your Azure VNETs via IPSEC VPNs over the Internet. This forms part of an SDWAN solution.

The service is built upon Microsoft's existing global network, for which here are some statistics/facts (correct at time of writing):

- Available in 54 Azure regions
- 100K+ Miles of fibre & subsea cables
- 130+ edge sites
- 200+ ExpressRoute partners

The above statistics/facts are only ever increasing as Microsoft invest more and more into their global network.

![Azure Global Network](/img/az-global-network.png)

Microsoft is putting in a lot of effort to partner with practically any vendor for this product. Currently they have partnered with the following vendors to offer simple integration/setup/automation steps for their shared customers:

- Citrix
- Riverbed Technology
- Palo Alto Networks
- Barracuda Networks
- Check Point
- 128 Technology
- Netfoundry

However for my testing I actually used a Juniper SRX 210H2 firewall that I use at home; and yes I used BGP as well to fully test the service! And this also supports Microsoft statements in a couple of webinars that as long as the device can support IKEv1/v2, then you can start utilising the Virtual WAN service. (Obviously your setup/configuration may not be as simple, but if you can build a VPN on your device today, you should have no problems at all)

For those partnered devices the configuration file you download from the VWAN

At a high level, you need to do the following:

1. Create the Virtual WAN resource (available from the Azure Marketplace)
2. Create a Virtual Hub in your chosen region (currently only allowed 1 Virtual Hub per Azure Region)
3. Connect the Virtual Hub to your Azure VNETs (enabling BGP as a default I would say as it wont do any harm and is probably the best way to control routing/failover etc...)
4. Create a VPN Site for each of your branch offices (enabling BGP as well on the VPN Sites)
5. Associate your VPN Site with your chosen Virtual Hub
6. Configure your device at each VPN site to connect to the Virtual WAN service

![Virtual WAN Overview Diagram](/img/az-vwan-ovw.png)

At the moment this can only be done via the Azure Portal, PowerShell & Azure CLI, ARM templating is not currently supported, although I don't think it will be long before the template is available.

The service is available in all Azure public regions (so everywhere apart from the government ones!). And can support 20 Gbps throughput per Virtual WAN!

You can associate/connect VPN Sites to multiple Virtual Hubs if required to meet your topology requirements. However from what I have read and tested Virtual Hub to Virtual Hub connectivity is not possible, however I am not sure why this would be needed by anyone at this stage. But I'm sure there will come a day I wish this was possible!

## Limitations, Issues & Things Noted During Deployment

There are some limitations in place, although your deployments will have to be pretty large to hit these:

- 1000 connections per Virtual Hub
- 1 Virtual Hub per Region
- Existing VNETs to associate with cannot have an overlapping address space (obviously) and also cannot have any VPN Gateways in them.
  - I think this is because the Virtual Hub actually deploys a resource similar to a VPN Gateway and therefore you can only have one of those per VNET.
  - Also in terms of VNET Peering, which is how I believe from my own understanding the Virtual Hub to Site association works, you can only allow Gateway Transit on one side of the VNET Peering.
- From the Azure Portal when enabling BGP when creating a Virtual Hub you cannot change the BGP ASN from 65515
  - Although I suspect this can be changed via Resource Explorer or if configuring via Azure CLI or PowerShell (something I may get around to testing)
  - I also wonder how this may affect a VPN Site (branch) to Multi-Hub deployment topology from a routing perspective as the AS Paths would surely get in a muddle or it would have to go down to the lowest router ID or lowest neighbour address for path selection, which are never best to leave it too!
- When you download the VPN configuration file from the Virtual WAN dashboard, it creates a Blob Storage Account and creates a file within a container every time you select download on a VPN - Site portal page
  - Hidden cost, albeit it very small, that is worth knowing about

## Summary

Azure Virtual WAN I can see becoming a very favourable service/platform to use for lots of enterprises who like the OPEX costing model and also those who like to keep all their services in one portal.

The single dashboard to check the health of all of your sites, plus the integration with all of the other Azure services like Log Analytics, Azure Monitor and Network Watcher certainly makes this product a very attractive choice.

However it is early days for the product even though it's now GA, but there are already a handful of Preview features that will make this product very tough to beat in the future. Those features for those of you who are interested are:

- ExpressRoute integration between site using both ExpressRoute connections & VPN Connections
- ExpressRoute Direct support with Virtual WAN
- Client VPN option using OpenVPN into your Azure Virtual WAN Hubs
    - This alone could help a lot more business consolidate their remote access and security posture worries (I will certainly be keeping a close eye on this feature, watch out for a future blog post!)

As always comments are welcome below and please share this article if you have found it interesting or useful in any way.

**Thanks to Microsoft for their resources/diagrams that have assisted in creating this post**