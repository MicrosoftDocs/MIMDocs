---
# required metadata

title: Microsoft Identity Manager licensing and downloads
description: This article outlines the approaches for licensing Microsoft Identity Manager (MIM) 2016, with pointers on where to download the software.
keywords:
author: markwahl-msft
ms.author: mwahl

ms.date: 11/10/2024
ms.topic: article
ms.service: microsoft-identity-manager
ms.assetid: 

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: billmath
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Microsoft Identity Manager 2016 licensing and downloads

This article outlines the approaches for licensing Microsoft Identity Manager (MIM) 2016, with pointers on where to download the software.

## Licensing MIM for your organization

Microsoft Identity Manager 2016 is licensed on a per-user basis. The details on licensing are included in the Product Terms and related documents, which can be downloaded from the [licensing terms](https://www.microsoft.com/licensing/product-licensing/products.aspx) page.

<a name='licensing-for-azure-ad-premium-customers'></a>

### Licensing for Microsoft Entra ID P1 or P2 customers

Microsoft Identity Manager 2016 is included with Microsoft Entra ID P1 or P2 (P1 and P2), which is part of Enterprise Mobility + Security.

Microsoft Entra ID P1 or P2 is available through a [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx), the [Open Volume License Program](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx), and the Cloud Solution Providers program. Azure and Microsoft 365 subscribers can also buy Microsoft Entra ID P1 and P2 online. Read more at [Microsoft Entra pricing](https://www.microsoft.com/security/business/microsoft-entra-pricing).

### MIM CALs

If you do not have Microsoft Entra ID P1 or P2 subscriptions for your users, and are using more MIM capabilities beyond synchronization, then a [Client Access License (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) is required for each user whose identity is managed in MIM. If you want external users—such as business partners, external contractors, or customers—to be able to access MIM, you can acquire CALs for each of your external users, or acquire External Connector (EC) licenses. Microsoft Identity Manager 2016 CALs are not required for users whose identity is only in the Microsoft Identity Manager synchronization service and is not managed in any other MIM component.

### Licenses for platform components

A Windows Server license is required to use Microsoft Identity Manager 2016’s server software as a Windows Server add-on. And a MIM deployment also requires a SQL Server installation. Windows Server and SQL Server licenses are not included with MIM.

## Obtaining MIM software

Before starting a new install of MIM or an upgrade from an earlier version, ensure you have the latest versions.

If you are starting a fresh install, you will need to download the installation files for each MIM component that is relevant to your scenario. Then, download any updates for those files, and then download any additional components that are separate downloads from the Download Center.


| Scenario       | Component | Required for scenario? | DVD ISO folder name | Comments |
|:---------------|:-----------------------------------------|:----|:--------------------------|:----------|
| Synchronization | Sync Service (including connector to AD) | Yes | `Synchronization Service` | |
| Synchronization | PCNS                                     | No  | `Password Change Notification Service` |  To be installed on domain controllers |
| Synchronization | Connectors for LDAP, SQL, Web Services, PowerShell, Lotus Domino, Graph | No | N/A | Distributed via Download Center |
| Privileged Access Management | MIM Service | Yes | `Service and Portal` | |
| Self-service | MIM Service, MIM Portal | Yes | `Service and Portal` | |
| Self-service | Add-ins and extensions | No | `Add-ins and extensions` | To be installed on end-user PCs |
| Self-service | SCSM Reporting | No | `Data Warehouse Support Scripts` | |
| Self-service | Hybrid reporting agent | No | N/A | Distributed via Download Center |
| Self-service | Language packs | No | `LANGUAGE Packs` | |
| Certificate Management | CM                 | Yes | `Certificate Management` | |
| Certificate Management | CM Bulk Client     | No  | `CM Bulk Client`         | |
| Certificate Management | CM Client          | No  | `CM Client`              | |
| Certificate Management | CM App for Windows | No  | `FIMCMModernApp*`        | |

### Obtaining Windows installer packages

For a new installation, most organizations with Volume License agreements download the MIM installation packages from the [Microsoft 365 admin center](https://www.microsoft.com/licensing/servicecenter/default.aspx). The DVD ISO file contains one folder for each MIM component: `Synchronization Service`, `Service and Portal`, etc. If you are going to install the software on a different computer from which you downloaded it, be sure to copy either the entire ISO file or the folder for the component: do not merely copy just an MSI file out of a folder without the rest of the files and sub-folders.

If you do not have Volume Licensing and have a subscription for Microsoft Entra ID P1 or P2, you can download the [Microsoft Entra ID P1 or P2 edition of MIM 2016](https://aka.ms/MIMforAADP). This edition includes the `Synchronization Service` and `Service and Portal` components of MIM 2016 SP2. All the changes from published hotfixes as of March 2021 are included in the installers; later hotfixes must be downloaded separately. The MIM Service installer for the Microsoft Entra ID P1 or P2 edition, in order to validate your subscription, requires internet connectivity and will ask you to provide Microsoft Entra credentials with enough permissions to read subscribedSKUs from your directory.

If you do not have Volume Licensing, customers with an appropriate developer subscription can also download MIM 2016 SP2 as an ISO file from [Visual Studio My Benefits Downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=). Search for `Microsoft Identity Manager 2016 with Service pack 2`.

### Obtaining updates

After installing MIM from an MSI file, you should next install the necessary hotfixes.

Check the [Identity Manager version release history](./reference/version-history.md) for the most recent update release, which has a link to the download site for the installer patch files.

To determine which update files are necessary, this table lists the components and the name of the corresponding patch (MSP) file in an update.

| Scenario | Component | DVD ISO folder name | Corresponding update patch file name |
|:----------|:-----------|:-------------------|:----------|
|Synchronization| Sync Service | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Self-service | MIM Service, MIM Portal | `Service and Portal`     | `MIMService_x64*msp` |
| Self-service | Add-ins and extensions  | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Self-service | Language packs          | `LANGUAGE Packs`         | `LANGUAGE Packs.zip` |
| Access management (BHOLD) | BHOLD          | `BHOLD`                   | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Certificate Management    | CM             |  `Certificate Management` | `MIMCM*.msp` |
| Certificate Management    | CM Bulk Client |  `CM Bulk Client`         |`MIMCMBulkClient*msp` |
| Certificate Management    | CM Client      | `CM Client`               |`MIMCMClient*msp` |

Be sure to read any release notes associated with the update prior to installing the MSP file.

Updates to [BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) are not distributed as MSP files, only as MSI installers.

### Other downloads

The following downloads may also be relevant:

- [Generic LDAP Connector, Generic SQL Connector, Graph Connector, Lotus Domino Connector, PowerShell Connector, Web Services Connector](https://go.microsoft.com/fwlink/?LinkId=717495)

- [Connector for SharePoint User Profile Store](https://www.microsoft.com/download/details.aspx?id=41164)


## Next steps

- Learn more on scenarios delivered in [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Read the [capacity planning guide](capacity-planning-guide.md).
- Deploy MIM for a [synchronization scenario](microsoft-identity-manager-deploy.md).
