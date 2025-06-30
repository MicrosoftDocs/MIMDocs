---

title: Microsoft Identity Manager | Microsoft Docs
description: MIM includes the access management capabilities of MIM 2016 and helps you manage users, credentials, policies, and access within your organization.

services: active-directory
documentationcenter: ''
keywords: MIM
author: billmath
reviewer: markwahl-msft, Dickson-Mwendia, henrymbuguakiarie
manager: benyim

ms.assetid: b0b39631-66df-4c5f-90c9-a1774346f816
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.service: entra-id-governance
ms.subservice: ''
ms.date: 06/30/2025
ms.author: billmath
ms.reviewer: mwahl, dmwendia, henrymbugua
ms.suite: ems
---

# Microsoft Identity Manager 2016 news and updates

Microsoft Identity Manager (MIM) 2016 builds on the identity and access management capabilities of Forefront Identity Manager (FIM) 2010 and earlier technologies. MIM integrates with different platforms across the datacenter, including on-premises HR systems, directories, and databases.

MIM works with Microsoft Entra cloud-hosted services to help your organization keep the right users in Active Directory for on-premises apps. Microsoft Entra Connect then syncs those users to Microsoft Entra ID for Microsoft 365 and cloud-hosted apps. Common MIM scenarios include:
  - Automatic identity and group provisioning based on business policy and workflow-driven provisioning
 - Integration of directory contents with HR systems and other sources of authority
 - Syncing identities between directories, databases, and on-premises apps through common APIs and protocols, Microsoft-delivered connectors, and partner-delivered connectors

Microsoft regularly releases updates to MIM, including enhancements based on customer requests and bug fixes, through hotfixes and service packs. The current MIM releases, MIM 2016 Service Pack 3 (SP3), and later hotfixes are supported under both fixed and Azure support policies. See the [version history](./reference/version-history.md) for links to the most recent updates. If you're running FIM, MIM 2016 SP2, or earlier versions, we recommend installing the latest MIM 2016 SP3 version.


## Updates in MIM 2016 SP3

MIM Service Pack 3 brings significant updates to the Microsoft Identity Manager ecosystem, including enhanced functionality and compatibility with more platforms and services. This section highlights the main improvements and deprecated features in this release.

#### Feature improvements

- **MIM Synchronization Service**

This release lets you install MIM Synchronization Service on Windows Server 2025. The synchronization engine works with SQL Server 2022 and Exchange Server 2019. You can also use Azure SQL as a backend database, with authentication using system-assigned or user-assigned managed identities. 

- **MIM Service and Portal**

The latest MIM 2016 SP3 release allows you to deploy the MIM Service and Portal on Windows Server 2025. This release includes compatibility with SQL Server 2022, Exchange Server 2019, SharePoint Server Subscription Edition (SE), and System Center Service Manager Data Warehouse 2022. [MIM 2016 SP3 also supports Active Directory Federation Services (ADFS)](mim-adfs-prepare-installation.md), enabling claims-based single sign-on (SSO) functionality. 

- **Software prerequisites** 

Before setup, install Visual C++ 2013 Redistributable Packages and either .NET Framework 4.6 or 4.8. The MIM Synchronization Service requires Microsoft OLE DB Driver 19 on the server where you host it. The MIM Service component requires .NET Framework 3.5 on the host server. 

#### Deprecated features

- **ECMA1 management agent framework** 

Microsoft doesn't recommend creating new management agents using the ECMA1 extensibility framework. This framework is replaced by ECMA 2.0, which provides a more modern, robust, and supported foundation for building custom connectors. 

If you use ECMA1-based agents, start planning a migration to ECMA2. ECMA2 supports modern .NET development, including asynchronous operations and improved error handling. ECMA1-based agents might stop working correctly with future releases or hotfixes. 

- **Azure Multi-Factor Authentication Server**

Deployments of Azure MFA Server no longer process MFA requests. If you use Azure MFA Server with MIM, for example, to secure self-service password reset (SSPR) or MIM PAM approvals, transition to supported alternatives such as Custom MFA providers, smartcard-based authentication, and Windows Hello for Business.

Cloud-based Azure AD MFA isn't directly integrated with MIM workflows, but consider it as part of broader identity modernization strategies. 

- **Connectors and Management Agents**

    - Forefront Identity Manager Certificate Management (FIM CM) is deprecated. Use modern certificate lifecycle tools like Microsoft Intune or Azure Key Vault.
    - Lotus Notes Management Agent (MA) is deprecated. Consider moving to a supported collaboration platform or developing a custom ECMA2 connector if you need to continue using it.
    - SAP R/3—The SAP R/3 MA is deprecated. Use the SAP NetWeaver connector or ECMA2-based integration for S/4HANA and modern SAP environments. 

## Updates in MIM 2016 SP2

MIM 2016 Service Pack 2 is a rollup of existing hotfixes since MIM 2016 SP1. It also introduces the option to configure use of Group Managed Service Accounts for MIM Synchronization Service and MIM Service, and enables MIM to be deployed with other updated platform software. More details could be found in [MIM 2016 Version Release History](./reference/version-history.md).

<a name='support-update-for-azure-active-directory-premium-customers'></a>

### Support update for Microsoft Entra ID P1 or P2 customers

The end of support date for Microsoft Identity Manager 2016 has been extended from January 13, 2026 to January 9, 2029.

For Microsoft Entra ID Premium customers, standard support continues to be available for customers using the [MIM for Microsoft Entra Premium customers](https://aka.ms/MIMforAADP) release (or the current MIM hotfix) to prepare data for AD that can then be sent to Microsoft Entra ID. For more information, see the [Microsoft Entra support process](support-update-for-azure-active-directory-premium-customers.md).

### Deprecations of other Microsoft components impacting MIM

 - Microsoft Entra Multifactor Authentication Server is deprecated, and beginning September 30, 2024, Microsoft Entra Multifactor Authentication Server deployments no longer services multifactor authentication (MFA) requests. Customers of Microsoft Entra Multifactor Authentication Server, for MIM SSPR or MIM PAM approvals, must move to instead use either custom MFA providers, or Windows Hello or smartcard-based authentication in AD.
 - Microsoft Silverlight is no longer available for download and is at [end of support](https://support.microsoft.com/windows/silverlight-end-of-support-0a3be3c7-bead-e203-2dfd-74f0a64f1788).  Customers with an existing BHOLD deployment of one or more of those modules with a Silverlight dependency should plan to uninstall those modules from their BHOLD server computers and uninstall Silverlight from any user computers that were previously interacting with that BHOLD deployment.
 - The MIM hybrid reporting feature, introduced with Microsoft Identity Manager (MIM) 2016, is deprecated, and replaced by using Azure Arc agent to send  event logs to Azure Monitor, as this allows more flexible reports. As of November 2025, the cloud endpoints used by the MIM hybrid reporting agent will no longer be available, and customers should transition to Azure Monitor or similar. For more information, see [Microsoft Identity Manager 2016 reporting with Azure Monitor](mim-azure-monitor-reporting.md).

### Major new and updated scenarios in MIM

- [Generic CSV Connector](./reference/microsoft-identity-manager-2016-connector-genericcsv.md) last updated March 2024
- [MIM deprecated feature list and planning for the future](microsoft-identity-manager-2016-deprecated-features.md), last updated October 2022
- [Microsoft Entra B2B collaboration with MIM Graph connector and Azure Application proxy is GA](microsoft-identity-manager-2016-graph-b2b-scenario.md), last updated December 2020

### Recent software releases

- [MIM for Microsoft Entra ID P1 or P2 customers](https://aka.ms/MIMforAADP), last updated June 2021
- [MIM Sync, Service, Portal, CM, Add-ins and client releases](./reference/version-history.md) last updated October 2023
- [MIM Connector releases](./reference/microsoft-identity-manager-2016-connector-version-history.md), last updated March 2024
- [MIM BHOLD modules releases](./reference/version-bhold-history.md) last updated October 2018


## Related articles

- Learn more about scenarios added in MIM 2016 and earlier at [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Read more documentation on deploying MIM and the latest version at the [MIM Documentation Roadmap](/microsoft-identity-manager/).
