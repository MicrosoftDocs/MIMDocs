---
# required metadata

title: Deploy MIM 2016 | Microsoft Identity Manager
description: Get the full list of steps involved in deploying Microsoft Identity Manager 2016, from preparing the environment to configuring the portals.
keywords:
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Deploy MIM 2016
The articles in this section provide step-by-step instructions for deploying Microsoft Identity Manager (MIM) 2016 for end-user self-service scenarios on a fresh server that has not had FIM or MIM previously deployed.

> [!NOTE]
> The deployment topology described in this section is intended for only for getting started and learning about MIM.  The [capacity planning guide](/microsoft-identity-manager/plan-design/capacity-planning-guide) provides more information on topologies for production deployments.  We recommend reviewing that documentation before deploying MIM for production scale or use.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## First: Prepare a domain
MIM works with Active Directory (AD), so follow these steps to configure your AD domain controller.
- [Domain setup](preparing-domain.md)

## Next: Prepare an identity management server
Once your domain is in place and configured, prepare your corporate identity management server. This includes setting up:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (optional)

## Finally: Install Microsoft Identity Manager 2016 components
Once you have set up the domain and server, you're ready to install the MIM components and configure them to sync with AD.
- [MIM Synchronization Service](install-mim-sync.md)
- [MIM Service and Portal](install-mim-service-portal.md)
- [Synchronize Active Directory and MIM Service databases](install-mim-sync-ad-service.md)
