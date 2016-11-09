---
# required metadata

title: Deploy and configure PAM | Microsoft Identity Manager
description: The roadmap for installing and MIM and configuring it for Privileged Access Management.
keywords:
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
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

# Configure the MIM environment for Privileged Access Management
There are seven steps to complete when setting up the environment for cross-forest access, installing and configuring Active Directory and Microsoft Identity Manager, and demonstrating a just-in-time access request.

These steps are laid out so that you can start from scratch and build a test environment. If you're applying PAM to an existing environment, you can use your own domain controllers or user accounts instead of creating new ones to match the examples.

1.  Prepare *CORPDC* server as a domain controller and *CORPWKSTN* as a member workstation.

2.  Prepare *PRIVDC* server as a domain controller.

3.  Prepare *PAMSRV* server in the *PRIV* forest.

4.  Install MIM components on *PAMSRV* and the cmdlets on a *CONTOSO* forest member workstation, and prepare them for Privileged Access Management.

5.  Establish trust between *PRIV* and *CONTOSO* forests.

6.  Preparing privileged security groups with access to protected resources and member accounts for Just-in-time Privileged Access Management.

7.  Demonstrate requesting, receiving, and making use of privileged elevated access to a protected resource.

>[!div class="step-by-step"]
[Start »](step-1-prepare-corp-domain.md)
