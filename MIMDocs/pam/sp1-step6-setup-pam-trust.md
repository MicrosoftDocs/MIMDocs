---
title: Step 6 Setup the PAM trust
description: Prepare the CORP domain with existing or new identities to be managed by Privileged Identity Manager using scripts
keywords:
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Setup the PAM trust

**This is not required for a PRIV only environment**
Login to the PAMServer with the MIMAdmin account.

  * Login to the PAMServer with the MIMAdmin Account
  * Run PowerShell as administrator
  * cd $env:SYSTEMDRIVE\PAM
  * .\PAMDeployment.ps1
  * select Menu option 6 (PAM trust setup)

  When prompted, enter the credentials for the CORP admin account. After providing credentials, the trust will be established and the configuration is complete.
