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
ms.date: 03/17/2021
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

Microsoft Identity Manager (MIM) 2016 builds on the identity and access management capabilities of Forefront Identity Manager. Like its predecessor, MIM helps you manage the users, credentials, policies, and access within your organization.  Additionally, MIM 2016 adds a hybrid experience, privileged access management capabilities, and support for new platforms.


With MIM, an organization can simplify identity lifecycle management with automated workflows, business rules and easy integration with heterogeneous platforms across the datacenter. MIM enables the organization to have the right users and access rights for Active Directory for on-premises apps, and Azure AD Connect can then make available in Azure AD for Microsoft 365 and cloud-hosted apps. Common MIM scenarios include:
 - Automatic identity and group provisioning based on business policy and workflow-driven provisioning
 - Integration of the contents of directories with HR systems and other sources of authority
 - Synchronizing identities between directories, databases, and on-premises applications through common APIs and protocols, Microsoft-delivered connectors, and partner-delivered connectors

The current MIM releases supported under fixed and Azure support policies are MIM 2016 Service Pack 2 (SP2) and later hotfixes.  Customers running FIM or MIM versions prior to MIM 2016 SP2 should upgrade to MIM 2016 SP2 or a later hotfix.

Microsoft regularly delivers updates to MIM, including enhancements for customer requests and bug fixes, on an ongoing release cycle through hotfixes and service packs. See the [version history](./reference/version-history.md) for links to the most recent.

## Updates in MIM 2016 SP2

MIM 2016 Service Pack 2 is a rollup of existing hotfixes since MIM 2016 SP1. It also introduces the option to configure use of Group Managed Service Accounts for MIM Synchronization Service and MIM Service, and enables MIM to be deployed with other updated platform software. More details could be found in [MIM 2016 Version Release History](./reference/version-history.md).

### Support update for Azure Active Directory Premium customers
For Azure AD Premium customers, standard support is available from June 2020 onward, continuing after January 2021. For more information, see the [Azure AD support process](support-update-for-azure-active-directory-premium-customers.md).

### Major new and updated scenarios

- [Azure AD B2B collaboration with MIM Graph connector and Azure Application proxy is GA](microsoft-identity-manager-2016-graph-b2b-scenario.md), last updated July 2019
- [Hybrid MIM reporting is GA](https://cloudblogs.microsoft.com/enterprisemobility/2018/02/23/hybrid-mim-reporting-now-available-in-azure-active-directory/), last updated April 2019
- [MIM deprecated feature list revised](microsoft-identity-manager-2016-deprecated-features.md), last updated February 2018

### Recent software releases

- [MIM for Azure AD Premium customers](https://aka.ms/MIMforAADP), last updated March 2021
- [MIM Sync, Service, Portal, CM, Add-ins and client releases](./reference/version-history.md) last updated March 2021
- [MIM Connector releases](./reference/microsoft-identity-manager-2016-connector-version-history.md), last updated March 2021
- [MIM BHOLD modules releases](./reference/version-bhold-history.md) last updated October 2018


## Related topics

Learn more on scenarios added in MIM 2016 and earlier at [Microsoft Identity manager 2016](microsoft-identity-manager-2016.md).

Read more documentation on deploying MIM and the latest version at the [MIM Documentation Roadmap](https://docs.microsoft.com/microsoft-identity-manager/).

