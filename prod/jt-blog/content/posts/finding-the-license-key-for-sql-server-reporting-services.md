---
title: "Finding the License Key for SQL Server Reporting Services"
summary: Ever wondered how to get the License Key to install SSRS on an IaaS VM in Azure when using PAYG licensing from Azure.
date: 2019-03-24
draft: false
categories: ["SQL"]
tags: ["Azure", "Gotchas", "How To", "IaaS", "Licensing", "SQL", "SSRS"]
thumbnail: "/img/thumbs/ssrs-thumb.png"
url: "/sql/finding-the-license-key-for-sql-server-reporting-services"
---

A very common question I seem to get from customers, colleagues & friends in the Azure community is “Where do I find the product key for SQL Server Reporting Services, if I’m using PAYG licensing from Azure?”.

And if I’m honest this is a very good question, as you never get shown the product key in the Azure portal when deploying and there is no command you can run via PowerShell or AZ CLI to get the key.

However finding the key is actually very easy and isn’t just applicable to Azure, so you can use option 1 below on any SQL server to find the product key.

I’ll assume you know how to download and install the latest version of SSRS. But here is a handy link to the download page if not. [Download SSRS 2017.](https://www.microsoft.com/en-us/download/details.aspx?id=55252)

So lets get into the 2 methods you can use to find the product key on an Azure IaaS SQL VM deployed using a marketplace image on the PAYG licensing model.

## Option 1 – Using The SQL Server Setup Wizard

1. Connect to and login to your newly deployed SQL IaaS VM via RDP
2. Open Windows Explorer and navigate to the following path: ‘C:\SQLServerFull\’
   ![SQL Server Media Path On Azure a VM](/img/iaas-sql-path.png)

3. Locate ‘Setup.exe’ and double click on it to launch the setup wizard
    ![SQL Server Setup](/img/iaas-sql-setup.png)
    ![SQL Server Installation Center](/img/iaas-sql-install-center.png)

4. Select the ‘Maintenance’ pane from the left hand side menu and then click on ‘Edition Upgrade’
    ![SQL Server Edition Upgrade](/img/iaas-sql-key.png)

5. The ‘Edition Upgrade’ wizard will launch after a few moments and will display a product key
   ![SQL Server Product Key](/img/iaas-sql-key.png)

This is the product key that you require and has been used to install/license the SQL Database Engine that is running on this VM. Copy this key to a notepad file and then cancel all of the wizards and close the setup launcher.

Then launch your SSRS installation wizard and use the key you have copied to a notepad file.

## Option 2 – Extract Key From DeafultSetup.ini

1. Connect to and login to your newly deployed SQL IaaS VM via RDP
2. Open Windows Explorer and navigate to the following path: ‘C:\SQLServerFull\x64’
    ![DefaultSetup.ini Path](/img/iaas-sql-defaultini-location.png)

3. Locate a file called ‘DefaultSetup.ini’ it may just be shown as ‘DefaultSetup’ if you don’t have file extensions shown. Double click on this file and open it with Notepad.
    ![Open DefaultSetup.ini with Notepad](/img/iaas-open-defaultini-notepad.png)

4. Notepad will then display the contents of the .ini file and within this the product key is shown next to “PID=”
    ![DefaultSetup.ini Product Key](/img/iaas-defaultini-key.png)

Again this is the product key that you require and has been used to install/license the SQL Database Engine that is running on this VM. Copy this key to a notepad file and then close the ‘DefaultSetup.ini’ file without making any changes to it and saving them.

Then launch your SSRS installation wizard and use the key you have copied to a new notepad file.

## Summary

Hopefully this article will help you all out at some point in the future. It’s a curve ball that’s come my way a few times and took me a bit of research to find the above methods.

Until next time!