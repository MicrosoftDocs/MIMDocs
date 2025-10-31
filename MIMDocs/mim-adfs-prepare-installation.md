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

Microsoft Identity Manager (MIM) works with Active Directory Federation Services (ADFS) to provide secure authentication using claims-based identity. This article shows you how to set up authentication in the MIM portal with ADFS claims.

## Topology

This article uses a two-server topology. One server, DC, acts as the domain controller, and the other, MIM (domain joined), hosts all other primary applications and services. This topology isn't a recommended production setup. It's only for a demonstration lab to show claims-based authentication with ADFS on the MIM portal.

| Server        | Operating system    | Hardware               | Functions                                                                   |
|---------------|---------------------|------------------------|-----------------------------------------------------------------------------|
| DC            | Windows Server 2022 | RAM: 8 GB, HDD: 127 GB  | Domain controller, DNS server, CA                                           |
| MIM           | Windows Server 2022 | RAM: 12 GB, HDD: 200 GB | ADFS server, SQL Server, SharePoint Server, MIM Sync, MIM Service, and Portal |

This article uses **contoso.com** as the domain and **CONTOSO** as the NETBIOS name. Replace these placeholders with your own domain values wherever they appear.

## Software requirements

1. Windows Server 2022
1. SQL Server 2022
1. SharePoint Subscription Edition
1. MIM SP2 for MIM Sync Engine
1. MIM SP3 build for the MIM Service and Portal

## Prerequisites

1. Install and set up the DNS role.
1. Install and set up the Active Directory Certificate Services (AD CS) role with the CA service.
1. Create relevant service accounts and apply appropriate access policies.
1. Install SharePoint Subscription Edition. You set up everything later.
1. Install SQL Server and set up security accounts.
1. Install the MIM Synchronization Service.

This ADFS installation guide assumes that you have prior experience with installing the MIM Service and Portal. For a refresher, refer to the [MIM deployment guide](microsoft-identity-manager-deploy.md).

## Prepare ADFS installation for the DC Server

### Add a DNS Host Record for ADFS

1. Open the DNS Management tool.
1. In the left navigation pane, expand:
   - Your computer's name
   - Forward Lookup Zones
   - Your domain

1. **Example:** DC > Forward Lookup Zones > contoso.com
1. In the main content area, select **New Host (A or AAAA)...**
1. In the dialog that appears, enter the following details:

   - **Name:** adfs
   - **FQDN:** adfs.contoso.com
   - **IP Address:** 10.2.200.200 (Replace this value with your server's actual IP address)

1. Select **Add Host** to create the record, then select **OK** to dismiss the notification dialog, and finally **Done**.
1. Close the DNS Management tool.

### Create a certificate template for ADFS

1. Open the Certificate Authority tool.
1. In the navigation pane, expand The CA name (contoso-DC-CA) and select **Certificate Templates**.
1. On the menu that appears, select **Manage**. The Certificate Templates Console opens.
1. Select the Web Server template and then **Duplicate Template**
1. The properties window opens, which allows us to customize the ADFS certificate.
1. Configure the certificate's properties as follows:

   - **General**
     - **Template display name:** ADFS Certificate
     - **Template name:** ADFSCertificate
     - **Publish certificate in Active Directory:** True (check the box)
   - **Request Handling**
     - **Allow private key to be exported:** True (check the box)
   - **Security**
     - Under security, select **Add** > **Object Types**.
     - Make sure Computers is selected in the list, and then select **OK**.
     - Find and add your computer to the list (for example, MIM), then select **OK**.
     - Assign your computer the permission to Enroll.
     - Select **Apply**, then select **OK** to close the properties window.

1. Close the Certificate Templates Console.
1. Select Certificate Templates, then select **New** > **Certificate Template to Issue**.
1. Select the ADFS Certificate you created, and then select **OK**.
1. Close the Certificate Authority tool.

## SharePoint Site Configuration - MIM Server

### Generate and install a self-signed certificate for MIM portal

1. Launch PowerShell as an administrator and run the following command:

   ```powershell
   New-SelfSignedCertificate -DnsName "mim.contoso.com" -CertStoreLocation "cert:\\LocalMachine\\My"
   ```

1. Right-click the start menu and select **Run**. In the dialog, enter **mmc** and select **OK**. This opens the **Microsoft Management Console**.
1. Select **File**, then select **Add/Remove Snap-in** to open the Snap-ins window.
1. Select **Certificates**, then select **Add**. In the window that appears, select the **Computer account** radio button, then select **Next**.
1. Select **Finish**, then select **OK**. The Certificates Snap-in is then available.
1. Expand the Personal folder and select Certificates. You see the certificate you generated.
1. Select your certificate, then select **Details** and **Copy to File…**. In the export wizard, select **Next**.
1. Select **No, do not export the private key**, then select **Next**.
1. On the file format page, keep the default **DER encoded binary X.509 (.CER)** option, then select **Next**.
1. Select **Browse** and save the certificate somewhere in your file system, for example `C:\\Users\\administrator.contoso\\Documents\\MIM\\MIMSSLCertificate.cer`
1. Select **Next**, then select **Finish**, **OK**, and **OK**.
1. Go to the certificate in your file system and select it to open its properties again.
1. On the General tab, select **Install Certificate**.
1. Select **Local Machine**, then select **Next**.
1. Select **Place all certificates in the following store**, then select **Browse**.
1. Select the **Trusted Root Certificate Authorities** store, then select **OK**, then select **Next**.
1. Select **Finish**. When the import is successful, select **OK** on the notification dialog. Select **OK** to close the properties dialog.
1. To verify the certificate, select it again, go to **Certification Path**, and check that the status is **OK**.

### Prepare SharePoint to host the MIM portal

#### Create a managed pool account

1. Launch the SharePoint Management Shell as an administrator and run the following commands:

   ```powershell
   $cred = Get-Credential
   ```

1. In the credentials dialog, enter your managed pool account username and password, then select **OK**.
1. Run the following command:

   ```powershell
   New-SPManagedAccount -Credential $cred
   ```

#### Create a SharePoint web application

1. On the SharePoint Management Shell, run the following commands to create a SharePoint web application:

   ```powershell
   $name = "MIM Portal"
   $url = "https://mim.contoso.com"
   $appPoolAccount = (Get-SPManagedAccount "CONTOSO\\svcMIMAppPool")
   New-SPWebApplication -Name $name -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $appPoolAccount -Port 443 -SecureSocketsLayer -Url $url
   ```

#### Create a SharePoint site

1. Create a SharePoint site by running the following command  in the SharePoint Management Shell:

   ```powershell
   New-SPSite -Name $name -URL $url -OwnerAlias "CONTOSO\\Administrator" -Template "STS#0"
   ```

1. Run the following commands to prepare the site for MIM portal installation:

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

#### Configure IIS bindings

1. Open the **Internet Information Services (IIS) Manager**.
1. In the navigation panel, expand your server name, then expand the **Sites** folder and select **MIM Portal**.
1. Under **Actions**, select **Bindings**.
1. Select the "https 443" site binding and then **Edit…**.
1. At the bottom of the Edit Site Binding form, select the SSL certificate dropdown and select the certificate you created earlier, `mim.contoso.com`.
1. Select **OK**, then close the IIS Manager.
1. Open Microsoft Edge and go to https://mim.contoso.com. When prompted for credentials, enter the credentials you used to create the site.

This sets up a SharePoint web application and site for the MIM portal. In the next how-to guide, you install and set up ADFS, then link it to your site.

## Next steps

- Learn how to [install and set up ADFS for MIM Server](mim-adfs-installation-configuration.md)