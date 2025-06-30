---
title: How to set up ADFS claims-based authentication in MIM
description: Secure MIM with ADFS - Learn how to implement claims-based authentication in the MIM portal using Active Directory Federation Services. Start configuring using this guide.
services: active-directory
author: Dickson-Mwendia
ms.author: dmwendia
reviewer: markwahl-msft, henrymbuguakiarie
ms.date: 06/30/2025
ms.topic: how-to
ms.service: microsoft-identity-manager
---

# How to authenticate in the MIM portal using ADFS claims

Microsoft Identity Manager (MIM) works with Active Directory Federation Services (ADFS) to provide secure authentication using claims-based identity. This article explains how to set up authentication in the MIM portal with ADFS claims.

## Topology

In this guide, we implement a two-server topology. One server, DC, functions as the Domain Controller and the other, MIM (domain joined), hosts all other primary applications & services. This topology isn't a recommended production architecture but is intended solely as a demonstration lab to showcase Claims-Based Authentication functionality with ADFS on the MIM Portal.

| Server        | Operating System    | Hardware               | Functions                                                                   |
|---------------|---------------------|------------------------|-----------------------------------------------------------------------------|
| DC            | Windows Server 2022 | RAM: 8GB, HDD: 127GB  | Domain controller, DNS server, CA                                           |
| MIM           | Windows Server 2022 | RAM: 12GB, HDD: 200GB | ADFS Server, SQL Server, SharePoint Server, MIM Sync, MIM Service and Portal |

This article uses **contoso.com** as the domain and **CONTOSO** as the NETBIOS name. Replace these placeholders with your domain-specific values wherever they appear.

## Software requirements

1. Windows Server 2022
1. SQL Server 2022
1. SharePoint Subscription Edition
1. MIM SP2 for MIM Sync Engine
1. MIM SP3 build for MIM Service and Portal

## Prerequisites

1. DNS role installed and configured.
1. Active Directory Certificate Services (AD CS) role with CA service installed and configured.
1. Relevant service accounts created with appropriate access policies applied.
1. Fresh installation of SharePoint Subscription Edition (we'll configure everything).
1. SQL Server installed with security accounts configured.
1. MIM Synchronization Service installed.

This ADFS installation guide assumes that you've got prior experience with installing the MIM Service and Portal. For a refresher, refer to the [MIM deployment guide](microsoft-identity-manager-deploy.md).

## Prepare for ADFS Installation - DC Server

### Add a DNS Host Record for ADFS

1. Open the DNS Management tool.
1. In the left-hand navigation pane, expand:

   - Your computer's name
   - Forward Lookup Zones
   - Your domain

1. **Example:** DC > Forward Lookup Zones > contoso.com
1. In the main content area, **right-click** and select **New Host (A or AAAA)...**
1. In the dialog that appears, enter the following details:

   - **Name:** adfs
   - **FQDN:** adfs.contoso.com
   - **IP Address:** 10.2.200.200 (Replace this with your server's actual IP)

1. Select **Add Host** to create the record then **OK** to dismiss the notification dialog and finally **Done**.
1. Close the DNS Management tool.

### Create a Certificate Template for ADFS

1. Open the Certificate Authority tool.
1. In the left-hand navigation pane, expand The CA name (contoso-DC-CA) and select **Certificate Templates**.
1. On the menu that appears, select **Manage**. This launches the Certificate Templates Console.
1. Right-click on the Web Server template and select **Duplicate Template**
1. This launches a properties window that we'll use to customize our ADFS certificate
1. Configure the certificate's properties as follows:

   - **General**
     - **Template display name:** ADFS Certificate
     - **Template name:** ADFSCertificate
     - **Publish certificate in Active Directory:** True (check the box)
   - **Request Handling**
     - **Allow private key to be exported:** True (check the box)
   - **Security**
     - Under security, select**Add** then **Object Types**.
     - Ensure Computers is selected in the list and then select **OK**.
     - Find and add your computer to the list (MIM in our case) then select **OK**.
     - Assign your computer the permission to Enroll
     - Select **Apply** then **OK**. This dismisses the properties window.

1. Close the Certificate Templates Console.
1. Right click on Certificate Templates once more and on the ensuing menu, hover over **New** to reveal a nested menu. Select **Certificate Template to Issue**
1. Select the ADFS Certificate you created and select **OK**
1. Close the Certificate Authority tool.

## SharePoint Site Configuration - MIM Server

### Generate & Install a Self Signed Certificate for MIM Portal

1. Launch PowerShell as an administrator and run the following command:

   ```powershell
   New-SelfSignedCertificate -DnsName "mim.contoso.com" -CertStoreLocation "cert:\\LocalMachine\\My"
   ```

1. Right click on the start menu and select**Run**. On the ensuing dialog, type **mmc** and then **OK**. This launches the **Microsoft Management Console**
1. Select **File** then **Add/Remove Snap-in**… to launch the Snap-ins window
1. Select**Certificates** then **Add**. On the window that pops up, select the **Computer account** radio button then **Next**
1. Select **Finish** then **OK**. The Certificates Snap-in should now be available.
1. Expand the Personal folder and Select Certificates. You should see the certificate you generated.
1. Select your certificate to view **Details** then **Copy to File…**. Select next on the export wizard.
1. Choose **No, do not export the private key** then select **Next**.
1. On the file format page, leave the default **DER encoded binary X.509 (.CER)** and select **Next**
1. Select **Browse** and save this certificate somewhere in your file system, for example `C:\\Users\\administrator.contoso\\Documents\\MIM\\MIMSSLCertificate.cer`
1. Select **Next** > **Finish** > **OK** > **OK**.
1. Navigate to this certificate in your file system and select it to launch its properties again.
1. On the General tab, select **Install Certificate**.
1. Select **Local Machine** then **Next**
1. Select **Place all certificates in the following store** then  **Browse**.
1. Select the **Trusted Root Certificate Authorities** store then **OK** and then **Next**
1. Select **Finish**. Once the import is successful, choose **OK** on the notification dialog. Select **OK** to dismiss the properties dialog.
1. You can verify that this certificate is OK by selecting it once more, navigating to **Certification Path** and verifying that the status is **OK**

### Prepare SharePoint to Host the MIM Portal

#### Create a Managed Pool Account

1. Launch the SharePoint Management Shell as an administrator and run the following commands:

   ```powershell
   $cred = Get-Credential
   ```

1. On the credentials dialog, provide your managed pool account username and password and select **OK**
1. Run the following command:

   ```powershell
   New-SPManagedAccount -Credential $cred
   ```

#### Create a SharePoint Web Application

1. On the SharePoint Management Shell, run the following commands to create a SharePoint web application:

   ```powershell
   $name = "MIM Portal"
   $url = "https://mim.contoso.com"
   $appPoolAccount = (Get-SPManagedAccount "CONTOSO\\svcMIMAppPool")
   New-SPWebApplication -Name $name -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $appPoolAccount -Port 443 -SecureSocketsLayer -Url $url
   ```

#### Create a SharePoint Site

1. Still on the SharePoint Management Shell, run the following command to create a SharePoint site:

   ```powershell
   New-SPSite -Name $name -URL $url -OwnerAlias "CONTOSO\\Administrator" -Template "STS#0"
   ```

1. Thereafter run the following commands to prepare the site for MIM Portal installation:

   ```powershell
   $w = Get-SPWebApplication https://mim.contoso.com/
   $w.BlockedASPNetExtensions.Remove("ashx")
   $w.Update()
   $contentService=[Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   $f = get-spfarm
   $f.update()
   iisreset
   ```

#### Configure IIS Bindings

1. Launch the **Internet Information Services (IIS) Manager**.
1. On the left navigation panel, expand the name of your server, then expand the **Sites** folder and select **MIM Portal**.
1. Select **Bindings** on the far-right panel under **Actions**.
1. Select the "https 443" site binding and click **Edit…**.
1. At the bottom of the Edit Site Binding form, there's an option to select an SSL certificate. Expand the dropdown and select the certificate we created earlier `mim.contoso.com`.
1. Select **OK** then **Close** the IIS Manager.
1. Launch Microsoft Edge and visit https://mim.contoso.com. When prompted for credentials, enter those you used to create the site.

This sets up a SharePoint web application and site for the MIM Portal. In the next how-to guide, we'll install and configure ADFS then link it to our site.

## Next steps

- Learn how to [install and configure ADFS for MIM Server](mim-adfs-installation-and-configuration.md)