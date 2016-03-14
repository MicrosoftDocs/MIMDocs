---
title: Deploying MIM
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
author: kgremban
---
# Deploying MIM
The articles in this section provide step-by-step instructions for deploying Microsoft Identity Manager (MIM) 2016 for end-user self-service scenarios on a fresh server that has not had FIM or MIM previously deployed.

> [!NOTE]
> The deployment topology described in this section is intended for only for getting started and learning about MIM.  The [capacity planning guide](/MIM/PlanDesign/capacity-planning-guide.html) provides more information on topologies for production deployments.  We recommend reviewing that documentation before deploying MIM for production scale or use.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## First: Prepare a Domain
MIM works with Active Directory (AD), so follow these steps to configure your AD domain controller.
- [Preparing a domain](preparing-domain.md)

## Then: Prepare an identity management server
Once your domain is in place and configured, prepare your corporate identity management server. This includes setting up
- [Preparing an identity management server: Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [Preparing an identity management server: SQL Server 2014](prepare-server-sql2014.md)
- [Preparing an identity management server: SharePoint](prepare-server-sharepoint.md)
- [Preparing an identity management server: Exchange](prepare-server-exchange.md) (optional)

## Finally: Install Microsoft Identity Manager 2016 components
Once your domain and server are set up, you're ready to install the MIM components and configure them to sync with AD.
- [Installing MIM 2016: MIM Synchronization Service](install-mim-sync.md)
- [Installing MIM 2016: MIM Service and Portal](install-mim-service-portal.md)
- [Installing MIM 2016: Synchronize Active Directory and MIM Service](install-mim-sync-ad-service.md)
