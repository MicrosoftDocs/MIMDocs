---

title: Microsoft Identity Manager | Microsoft Docs
description: MIM includes the access management capabilities of FIM 2010 and helps you manage users, credentials, policies, and access within your organization.
services: active-directory
documentationcenter: ''
keywords: MIM
author: EugeneSergeev
ms.author: esergeev
reviewer: markwahl-msft
manager: aashiman
ms.date: 08/6/2021
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity

ms.assetid: b0b39631-66df-4c5f-90c9-a1774346f816

ms.reviewer: mwahl
ms.suite: ems

---

# Microsoft Identity Manager 2016 news and updates

Microsoft Identity Manager (MIM) 2016 builds on the identity and access management capabilities of Forefront Identity Manager and predecessor technologies.  MIM provides integration with heterogeneous platforms across the datacenter, including on-premises HR, directories and databases.

MIM augments Azure AD cloud-hosted services by enabling the organization to have the right users in Active Directory for on-premises apps. Azure AD Connect can then make available in Azure AD for Microsoft 365 and cloud-hosted apps. Common MIM scenarios include:
 - Automatic identity and group provisioning based on business policy and workflow-driven provisioning
 - Integration of the contents of directories with HR systems and other sources of authority
 - Synchronizing identities between directories, databases, and on-premises applications through common APIs and protocols, Microsoft-delivered connectors, and partner-delivered connectors

Microsoft regularly delivers updates to MIM, including enhancements for customer requests and bug fixes, on an ongoing release cycle through hotfixes and service packs.  The current MIM releases, MIM 2016 Service Pack 2 (SP2) and later hotfixes, are supported under both fixed and Azure support policies.  See the [version history](./reference/version-history.md) for links to the most recent.  Customers running FIM or MIM versions prior to MIM 2016 SP2 should upgrade to the most recent hotfix of MIM 2016 SP2.

## Updates in MIM 2016 SP2

MIM 2016 Service Pack 2 is a rollup of existing hotfixes since MIM 2016 SP1. It also introduces the option to configure use of Group Managed Service Accounts for MIM Synchronization Service and MIM Service, and enables MIM to be deployed with other updated platform software. More details could be found in [MIM 2016 Version Release History](./reference/version-history.md).

### Support update for Azure Active Directory Premium customers
For Azure AD Premium customers, standard support continues to be available for customers using [MIM for Azure AD Premium customers](https://aka.ms/MIMforAADP) to prepare data for AD which can then be sent to Azure AD. For more information, see the [Azure AD support process](support-update-for-azure-active-directory-premium-customers.md).

### Major new and updated scenarios

- [MIM deprecated feature list and planning for the future](microsoft-identity-manager-2016-deprecated-features.md), last updated August 2021
- [Azure AD B2B collaboration with MIM Graph connector and Azure Application proxy is GA](microsoft-identity-manager-2016-graph-b2b-scenario.md), last updated December 2020
- [Hybrid MIM reporting is GA](https://cloudblogs.microsoft.com/enterprisemobility/2018/02/23/hybrid-mim-reporting-now-available-in-azure-active-directory/), last updated April 2019

### Recent software releases

- [MIM for Azure AD Premium customers](https://aka.ms/MIMforAADP), last updated March 2021
- [MIM Sync, Service, Portal, CM, Add-ins and client releases](./reference/version-history.md) last updated March 2021
- [MIM Connector releases](./reference/microsoft-identity-manager-2016-connector-version-history.md), last updated March 2021
- [MIM BHOLD modules releases](./reference/version-bhold-history.md) last updated October 2018


## Related topics

Learn more on scenarios added in MIM 2016 and earlier at [Microsoft Identity manager 2016](microsoft-identity-manager-2016.md).

Read more documentation on deploying MIM and the latest version at the [MIM Documentation Roadmap](https://docs.microsoft.com/microsoft-identity-manager/).

