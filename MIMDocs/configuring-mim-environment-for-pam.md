---
title: Configuring the MIM Environment for Privileged Access Management
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
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
author: Kgremban
---
# Configuring the MIM Environment for Privileged Access Management
There are seven steps to complete when setting up the environment for cross-forest access, installing and configuring Active Directory and Microsoft Identity Manager, and demonstrating a just-in-time access request.

1.  Prepare *CORPDC* server as a domain controller and *CORPWKSTN* as a member workstation.

2.  Prepare *PRIVDC* server as a domain controller.

3.  Prepare *PAMSRV* server in the *PRIV* forest.

4.  Install MIM components on *PAMSRV* and the cmdlets on a *CONTOSO* forest member workstation, and prepare them for Privileged Access Management.

5.  Establish trust between *PRIV* and *CONTOSO* forests.

6.  Preparing privileged security groups with access to protected resources and member accounts for Just-in-time Privileged Access Management.

7.  Demonstrate requesting, receiving, and making use of privileged elevated access to a protected resource.

The following sections provide details about how to perform these tasks.

