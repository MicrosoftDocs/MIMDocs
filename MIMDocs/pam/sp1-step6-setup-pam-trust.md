---
title: Step 6 Setup the PAM trust
description: Step 6 of configuring PAM using scripts. This section covers setting up the necessary trust between the corp and priv domains
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager

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

# Step 6 Set up the PAM trust

> [!div class="step-by-step"]
> [« Step 5](sp1-step5-configuring-pam.md)
> [Step 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**This is not required for a PRIV only environment**
Login to the PAMServer with the MIMAdmin account.

1. Login to the PAMServer with the MIMAdmin Account
2. Run PowerShell as administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. select Menu option 6 (PAM trust setup)

   When prompted, enter the credentials for the CORP admin account. After providing credentials, the trust will be established and the configuration is complete.

> [!div class="step-by-step"]
> [« Step 5](sp1-step5-configuring-pam.md)
> [Step 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
