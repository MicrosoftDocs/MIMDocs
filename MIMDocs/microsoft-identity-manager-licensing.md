---
# required metadata

title: Microsoft Identity Manager licensing and downloads | Microsoft Docs
description: This article outlines the approaches for licensing Microsoft Identity Manager (MIM) 2016, with pointers on where to download the software.
keywords:
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
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

Microsoft Identity Manager 2016 is licensed on a per-user basis.  The details on licensing are included in the Product Terms and related documents, which can be downloaded from the [licensing terms](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) page.

### Licensing for Azure AD Premium customers

Microsoft Identity Manager 2016 is included with Azure Active Directory Premium (P1 and P2), which is part of Enterprise Mobility + Security.

Azure AD Premium is available through a [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), the [Open Volume License Program](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), and the [Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) program. Azure and Office 365 subscribers can also buy Azure Active Directory Premium P1 and P2 online.  Read more at [Azure Active Directory pricing](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### MIM CALs

If you do not have Azure Active Directory Premium subscriptions for your users, and are using more MIM capabilities beyond synchronization, then a [Client Access License (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) is required for each user whose identity is managed in MIM. If you want external users—such as business partners, external contractors, or customers—to be able to access MIM, you can acquire CALs for each of your external users, or acquire External Connector (EC) licenses. Microsoft Identity Manager 2016 CALs are not required for users whose identity is only in the Microsoft Identity Manager synchronization service and is not managed in any other MIM component.

### Licenses for platform components

A Windows Server license is required to use Microsoft Identity Manager 2016’s server software as a Windows Server add-on. And a MIM deployment also requires a SQL Server installation.  Windows Server and SQL Server licenses are not included with MIM.

## Obtaining MIM software

Before starting a new install of MIM or an upgrade from an earlier version, ensure you have the latest versions.

If you are starting a fresh install, you will need to download the installation files for each MIM component that is relevant to your scenario. Then, download any updates for those files, and then download any additional components that are separate downloads from the Download Center.


| Scenario | Component | Required for scenario? | DVD ISO folder name | Comments |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synchronization| Sync Service (including connector to AD) | Yes | `Synchronization Service` | |
| Synchronization | PCNS | No | `Password Change Notification Service` |  To be installed on domain controllers |
| Synchronization | Connectors for LDAP, SQL, Web Services, PowerShell, Lotus Domino, Graph | No | N/A | Distributed via Download Center |
| Privileged Access Management | MIM Service | Yes | `Service and Portal` | |
| Self-service | MIM Service, MIM Portal | Yes | `Service and Portal` | |
| Self-service | Add-ins and extensions | No | `Add-ins and extensions` | To be installed on end-user PCs |
| Self-service | SCSM Reporting | No | `Data Warehouse Support Scripts` | |
| Self-service | Hybrid reporting agent | No | N/A | Distributed via Download Center |
| Self-service | Language packs | No | `LANGUAGE Packs` | |
| Certificate Management | CM | Yes | `Certificate Management` | |
| Certificate Management | CM Bulk Client | No | `CM Bulk Client` | |
| Certificate Management | CM Client | No | `CM Client`  | |
| Certificate Management | CM App for Windows | No | `FIMCMModernApp*` | | |

### Obtaining Windows installer packages

For a new installation, most organizations download the MIM installation packages from the [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


The DVD ISO file contains one folder for each MIM component: Synchronization Service, Service and Portal, etc. If you are going to install the software on a different computer from which you downloaded it, be sure to copy either the entire ISO file or the folder for the component: do not merely copy just an MSI file out of a folder without the rest of the files and sub-folders.

If you do not have access to the Volume Licensing Service Center, customers with an appropriate developer subscription can also download MIM 2016 SP1 as an ISO file from [Visual Studio My Benefits Downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Search for "Microsoft Identity Manager 2016 with Service Pack 1".  

If you do not have access to the Volume Licensing Service Center and merely wish to try out the MIM software for a limited time, you can download an [evaluation version of MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). This software is not intended for production use and will cease to operate 180 days after first installation, and cannot be upgraded. The evaluation version requires Windows Server 2008 R2, Windows Server 2012 or Windows Server 2012 R2 for installation.  If you are new to MIM and learning the technology, keep in mind that all MIM scenarios require an Active Directory domain, a Windows Server, and SQL Server to be present. If you do not have Windows Server or SQL Server already present, you may wish to try [provisioning a VM with SQL Server 2016 and Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### Obtaining updates

After installing MIM from MSI, you should next install the necessary hotfixes.

Check the [Identity Manager version release history](./reference/version-history.md) for the most recent update release, which has a link to the download site for the installer patch files.

To determine which update files are necessary, this table lists the components and the name of the corresponding patch (MSP) file in an update.

| Scenario | Component | DVD ISO folder name | Corresponding update patch file name |
|----------|-----------|-   |-------------------|----------|--------------|
|Synchronization| Sync Service | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Self-service | MIM Service, MIM Portal | `Service and Portal` | `FIMService_x64*msp` |
| Self-service | Add-ins and extensions | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Self-service | Language packs | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Access management (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Certificate Management | CM |  `Certificate Management` | `FIMCM*.msp` |
| Certificate Management | CM Bulk Client |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Certificate Management | CM Client | CM Client |`FIMCMClient*msp` |

Be sure to read any release notes associated with the update prior to installing the MSP file.

Updates to [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) are not distributed as MSP files, only as MSI installers.

### Additional downloads

The following downloads may also be relevant:

- [MIM Hybrid Reporting Agent](https://www.microsoft.com/download/details.aspx?id=55112)

- [Generic LDAP Connector, Generic SQL Connector, Graph Connector, Lotus Domino Connector, PowerShell Connector, Web Services Connector](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Connector for SharePoint User Profile Store](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- If you do not already have an Active Directory domain and are setting up a PAM scenario for experimentation, see the [MIM 2016 SP1 PAM deployment scripts](sp1-deployment-scripts.md).

## Next steps

- Learn more on scenarios delivered in [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Read the [capacity planning guide](capacity-planning-guide.md).
- Deploy MIM for a [synchronization scenario](microsoft-identity-manager-deploy.md) or the [Privileged access management scenario](./pam/privileged-identity-management-for-active-directory-domain-services.md).

