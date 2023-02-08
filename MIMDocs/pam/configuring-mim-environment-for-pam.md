---
# required metadata

title: Configure MIM 2016 for Privileged Access Management | Microsoft Docs
description: The roadmap for installing and MIM and configuring it for Privileged Access Management.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 01/27/2023
ms.topic: article
ms.prod: microsoft-identity-manager

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

These steps are laid out so that you can start from scratch and build a test environment. If you're applying PAM to an existing environment, you can use your own domain controllers or user accounts for the *CONTOSO* domain, instead of creating new ones to match the examples.

1. If you do not have an existing domain you wish to have as the domain to manage, prepare *CORPDC* server as a domain controller.

2. Prepare *PRIVDC* server as a domain controller for a separate WS 2016 domain and forest, *PRIV*.

3.  Prepare *PAMSRV* server in the *PRIV* forest, to hold the MIM server software.

4.  Install MIM components on *PAMSRV* and prepare them for Privileged Access Management.

5.  Install the cmdlets on a *CONTOSO* forest member workstation.

6.  Establish trust between *PRIV* and *CONTOSO* forests.

7.  Preparing privileged security groups with access to protected resources and member accounts for Just-in-time Privileged Access Management.

8.  Demonstrate requesting, receiving, and making use of privileged elevated access to a protected resource.

> [!div class="step-by-step"]
> [Start »](step-1-prepare-corp-domain.md)
