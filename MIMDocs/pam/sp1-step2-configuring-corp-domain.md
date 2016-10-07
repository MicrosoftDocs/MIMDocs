---
# required metadata

title: Step 2 Configuring the CORP domain
description: Prepare the CORP domain with existing or new identities to be managed by Privileged Identity Manager using scripts
keywords:
author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
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

# Step 2 Configuring the CORP domain

Once the SIDs.txt is copied to the CORPDC **not required for PRIVOnly deployments**

1. Login to CORPDC as Administrator
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. select Menu Option 2 (CORP Forest Configuration)

>[!div class="step-by-step"]
[« Step 1](sp1-step1-configuring-priv-domain.md)
[Step 3 »](sp1-step3-installing-configuring-sql.md)
