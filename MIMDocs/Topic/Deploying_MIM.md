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
This section provides step-by-step instructions for deploying Microsoft Identity Manager 2016 for two scenarios.

## MIM 2016 Deployment Options
If you're just getting started and want to learn about MIM, there are two common ways to deploy a MIM 2016 solution, both are described in this section:

-   Deploying MIM software for end-user self-service scenarios on a fresh server that has not had FIM or MIM previously deployed

-   Upgrading an existing FIM 2010 R2 test  system to MIM 2016

> [!NOTE]
> The deployment topology described in this section is intended for only for getting started and learning about MIM.  The [capacity planning guide](https://technet.microsoft.com/en-us/library/ff400279%28v=ws.10%29.aspx) provides more information on topologies for production deployments.  We recommend reviewing that documentation before deploying MIM for production scale or use.

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](../Topic/Getting_Started_with_Privileged_Access_Management.md).

