---

title: Microsoft Identity Manager
description: MIM includes the access management capabilities of FIM 2010 and helps you manage users, credentials, policies, and access within your organization.
services: active-directory
documentationcenter: ''
keywords: MIM
author: EugeneSergeev
ms.author: esergeev
reviewer: markwahl-msft
manager: benyim
ms.date: 10/23/2023
ms.devlang: na
ms.topic: article
ms.service: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity

ms.assetid: b0b39631-66df-4c5f-90c9-a1774346f816

ms.reviewer: mwahl
ms.suite: ems

---

# Microsoft Identity Manager 2016 news and updates

Microsoft Identity Manager (MIM) 2016 builds on the identity and access management capabilities of Forefront Identity Manager and predecessor technologies.  MIM provides integration with heterogeneous platforms across the datacenter, including on-premises HR systems, directories, and databases.

MIM augments Microsoft Entra cloud-hosted services by enabling the organization to have the right users in Active Directory for on-premises apps. Microsoft Entra Connect can then make available in Microsoft Entra ID for Microsoft 365 and cloud-hosted apps. Common MIM scenarios include:
 - Automatic identity and group provisioning based on business policy and workflow-driven provisioning
 - Integration of the contents of directories with HR systems and other sources of authority
 - Synchronizing identities between directories, databases, and on-premises applications through common APIs and protocols, Microsoft-delivered connectors, and partner-delivered connectors

Microsoft regularly delivers updates to MIM, including enhancements for customer requests and bug fixes, on an ongoing release cycle through hotfixes and service packs.  The current MIM releases, MIM 2016 Service Pack 2 (SP2) and later hotfixes, are supported under both fixed and Azure support policies.  See the [version history](./reference/version-history.md) for links to the most recent.  Customers running FIM or MIM versions prior to MIM 2016 SP2 should upgrade to the most recent hotfix of MIM 2016 SP2.

## Updates in MIM 2016 SP2

MIM 2016 Service Pack 2 is a rollup of existing hotfixes since MIM 2016 SP1. It also introduces the option to configure use of Group Managed Service Accounts for MIM Synchronization Service and MIM Service, and enables MIM to be deployed with other updated platform software. More details could be found in [MIM 2016 Version Release History](./reference/version-history.md).

<a name='support-update-for-azure-active-directory-premium-customers'></a>

### Support update for Microsoft Entra ID P1 or P2 customers

The end of support date for Microsoft Identity Manager 2016 has been extended from January 13, 2026 to January 9, 2029.

For Microsoft Entra ID P1 or P2 customers, standard support continues to be available for customers using the [MIM for Microsoft Entra ID P1 or P2 customers](https://aka.ms/MIMforAADP) release, or the current MIM hotfix, to prepare data for AD that can then be sent to Microsoft Entra ID. For more information, see the [Microsoft Entra ID support process](support-update-for-azure-active-directory-premium-customers.md).

### Deprecations of other Microsoft components impacting MIM

 - The Windows Azure AD Connector for FIM from 2014 is deprecated, and the Microsoft Entra internal interfaces used by that connector will be removed. Existing deployments should migrate to [Microsoft Entra Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect), Microsoft Entra Connect Sync, or the [Microsoft Graph Connector](microsoft-identity-manager-2016-connector-graph.md), as described in [how to migrate from the FIM Connector](migrate-from-the-fim-connector-for-azure-active-directory.md).
 - Azure Multi-Factor Authentication Server is deprecated, and beginning September 30, 2024, Azure Multi-Factor Authentication Server deployments will no longer service multifactor authentication (MFA) requests. Customers of Azure Multi-Factor Authentication Server, for MIM SSPR or MIM PAM approvals, should plan to move before this date to instead use either custom MFA providers, or Windows Hello or smartcard-based authentication in AD.
 - Microsoft Silverlight is no longer available for download and has reached [end of support](https://support.microsoft.com/windows/silverlight-end-of-support-0a3be3c7-bead-e203-2dfd-74f0a64f1788).  Customers with an existing BHOLD deployment of one or more of those modules with a Silverlight dependency should plan to uninstall those modules from their BHOLD server computers and uninstall Silverlight from any user computers that were previously interacting with that BHOLD deployment.

### Major new and updated scenarios in MIM

- [MIM deprecated feature list and planning for the future](microsoft-identity-manager-2016-deprecated-features.md), last updated October 2022
- [Microsoft Entra B2B collaboration with MIM Graph connector and Azure Application proxy is GA](microsoft-identity-manager-2016-graph-b2b-scenario.md), last updated December 2020

### Recent software releases

- [MIM for Microsoft Entra ID P1 or P2 customers](https://aka.ms/MIMforAADP), last updated June 2021
- [MIM Sync, Service, Portal, CM, Add-ins and client releases](./reference/version-history.md) last updated October 2023
- [MIM Connector releases](./reference/microsoft-identity-manager-2016-connector-version-history.md), last updated August 2023
- [MIM BHOLD modules releases](./reference/version-bhold-history.md) last updated October 2018


## Related topics

Learn more about scenarios added in MIM 2016 and earlier at [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).

Read more documentation on deploying MIM and the latest version at the [MIM Documentation Roadmap](/microsoft-identity-manager/).
