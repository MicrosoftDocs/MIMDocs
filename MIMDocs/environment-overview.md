---
# required metadata

title: Environment Overview | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Environment Overview
This test lab comprises software which can be installed on four physical or virtual machines, as illustrated in Figure 1. The remainder of these instructions assume the installation is performed on virtual machines which share a common private network (e.g., a virtual LAN with IP addresses numbered 10.0.x.x or 192.168.x.x).

> [!IMPORTANT]
> This CTP is not compatible with the database or directory contents from the previous CTP.  If you have been previously evaluating MIM for PAM or other scenarios, please back up and archive the virtual machines used for that test, and start the deployment with new virtual machine images that have not previously be used for MIM scenarios.

![](././media/pam-test-lab-architecture.png)
