---
title: "Using My Surface Go As A Second Screen"
summary: Working from home and need another screen? Or wanting to get a change of scenery from your normal home desk but need that second screen? This article is for you!
date: 2020-04-19
draft: false
categories: ["How To's"]
tags: ["How To's", "WFH", "Remote Working", "Working From Home", "Windows 10", "Miracast"]
thumbnail: "/img/thumbs/sfgo-screen-thumb.jpeg"
twitter:
  card: "summary"
  site: "@Jack_Ref"
  title: "Using My Surface Go As A Second Screen"
  description: "Working from home and need another screen? Or wanting to get a change of scenery from your normal home desk but need that second screen? This article is for you!"
  image: "https://jacktracey.co.uk/img/thumbs/sfgo-screen-thumb.jpeg"
---

So with the UK in lockdown due to COVID-19 along with the majority of the world, working from home is becoming the new normal for a lot of us. 

However our homes may not be somewhere we have ever had to work from, let alone have a decent IT setup like a second screen, desk phone, webcam, headset, etc. These things many of us now take for granted and have access to when working in our at our usual desks at our offices, but now we can't get to these, what can you do?

> First of all, a big shout out to [@Ryan_Littlemore](https://twitter.com/ryan_littlemore), who I now work with and run the [Sussex Azure User Group](https://sussexazure.uk) with, for telling me about this feature I hadn't even thought about!

![Surface Laptop 2 Miracasting To Surface Go](/img/sfgo-screen.jpeg)

## Introducing Miracast

Miracast is a wireless display protocol that is a published standard so all vendors can adopt, use and integrate the protocol into their products if they choose.

Luckily this is becoming a lot more popular with vendors and is included with pretty much every device you buy nowadays. Surface Laptop 2, Surface Go & my Samsung Smart LED TV all have the feature so I can use them all as an additional screen if I wanted to from any device that support projecting to a wireless display using the Miracast protocol!

> Miracast is also built into Windows 10 now so you can cast from all Windows 10 devices to a device that supports receiving Miracast traffic!

## How Do I Check My Device Supports Being Miracast Too?

For devices like TV's etc. honestly my best advice is to just try and see if the device appears when you try to cast from your Windows 10 device. As shown in Step 4 below.

## How Do I Miracast From Windows 10?

To Miracast from Windows 10 devices, do the following:

1. Press the following keys on your keyboard at the same time:{{< rawhtml >}} <kbd>Win</kbd>+<kbd>P</kbd> {{< /rawhtml >}}
2. The project pane should appear from the right hand side of the screen:
   
   ![Windows 10 Project](/img/windows-project.png)
3. Click on **"Connect to a wireless display"**
4. Then the **"Connect"** pane will appear and devices will start showing that are available to connect to both via Bluetooth (if applicable) and via Miracast for supported devices.
   
   ![Windows 10 Project - Connect](/img/windows-project-1.png)
   
   *As you can see my TV is showing as an available device, so lets connect to that!*
5. Click on the device you want to Miracast too and wait for it to connect. **You may need to accept the connection request on the receiving device, so lookout for that**
   
   ![Windows 10 Project - Connected](/img/windows-project-2.png)
6. As you can see I am now connected to my TV via Miracast, after accepting the connection request on my TV!
7. If you wish you can change the behaviour of the Miracast screen from "Duplicated" to "Extended" or another option by pressing {{< rawhtml >}} <kbd>Win</kbd>+<kbd>P</kbd> {{< /rawhtml >}} again and selecting the relevant option.
8. To disconnect you can just click the "Disconnect" button on the black ribbon at the top of your screen.
   
   ![Windows 10 Project - Disconnect](/img/windows-project-3.png)

Impressive, right!

![Surface Go Miracasting To TV](/img/sfgo-project-to-tv.jpeg)

## How Do I Setup Another Compatible Windows 10 Device To Support Receiving A Miracast From Another Device?

#### Checking Your Windows 10 Device Supports Being Miracasted Too

1. Press the following keys on your keyboard at the same time:{{< rawhtml >}} <kbd>Win</kbd>+<kbd>R</kbd> {{< /rawhtml >}}
2. Type the following into the Run window and hit enter: **"dxdiag"**
   
   ![DXDIAG](/img/mcast-support-1.png)
3. The **"DirectX Diagnostic Tool"** will appear, click on **"Save All Information..."** and save the .txt file to somewhere you can find it easily for the next step.
   
   ![DirectX Diagnostic Tool](/img/mcast-support-2.png)
4. Open the .txt file you saved from the previous step and look for the option call **"Miracast:"** in the file. As shown in green below:
   
   ![Miracast Option](/img/mcast-support-3.png)
5. If it states that it's **available** as shown in the screenshot above, you can use this device as an additional monitor to Miracast too from another Windows 10 device.

#### Configuring Your Windows 10 Device To Accept Incoming Miracast Connections

1. Open the Start Menu and type the following to start searching: **"projection"**
   
   ![Start Menu - Projection Settings](/img/mcast-setup-1.png)
2. Click on the **"Projection Settings"** option displayed from the search
3. Ensure the settings are configured as shown below (these are my preferred setting to ensure the most secure setup of Miracast):
   
   ![Miracast Projection Settings](/img/mcast-setup-2.png)
4. Your device can now be Miracast to from other devices that support Miracasting!

## Summary

So hopefully, like myself, you have now learnt the power of Windows 10 & Miracast support! 

In these strange times where working from home is the new normal for a lot of us, having the ability to use a second screen is vital to operating efficiently whilst "at work". Also if you cannot obtain a second physical monitor or you don't have the required connectors or cables to be able to use it from your device, this may help you out massively!

I'm going to be taking advantage of this a bit more, especially when I want to work from the garden a bit more in the better weather we are getting here in the UK, but I still need that second screen to work effectively.

Hope this helps a lot of you out there, feel free to share and comment below if this has helped you in anyway!