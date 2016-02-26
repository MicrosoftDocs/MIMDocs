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
author: Kgremban
---
# Deploying MIM
The articles in this section provide step-by-step instructions for deploying Microsoft Identity Manager (MIM) 2016 for end-user self-service scenarios on a fresh server that has not had FIM or MIM previously deployed.

> [!NOTE]
> The deployment topology described in this section is intended for only for getting started and learning about MIM.  The [capacity planning guide](https://technet.microsoft.com/library/ff400279.aspx) provides more information on topologies for production deployments.  We recommend reviewing that documentation before deploying MIM for production scale or use.

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).

## [Domain setup](preparing-domain.md)
MIM works with Active Directory (AD), so follow these steps to set up your AD domain if you haven't already.

## [Server setup](preparing-corporate-identity-management-server.md)
Use this article to deploy prepare your corporate identity management server. This includes Windows Server 2012 R2, SQL Server, and SharePoint.

## [Install MIM 2016 components](microsoft-identity-manager-2016-install-server-components.md)
Once your domain and server are set up, this article helps you get started with MIM services and configure them to sync with AD.
