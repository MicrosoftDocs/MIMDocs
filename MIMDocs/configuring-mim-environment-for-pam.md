---
# required metadata

title: Configuring the MIM Environment for Privileged Access Management | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

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
