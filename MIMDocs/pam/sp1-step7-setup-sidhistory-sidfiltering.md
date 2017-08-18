﻿---
title: Step 7 Setup SID history/SID filtering
description: This is Step 7 of configuring Privileged Identity Manager using scripts. This step covers setting up SID history/SID filtering.
keywords:
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
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

# Step 7 Set up SID history/SID filtering

>[!div class="step-by-step"]
[« Step 6](sp1-step6-setup-pam-trust.md)
[Step 8 »](sp1-step8-pam-deployment-verification.md)

**This is not required for a PRIV only environment**
Login to the PAMServer with the MIMAdmin account.

1. Login to the CORP DC as administrator
2. Run PowerShell as administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. select Menu option 8 (Setup SID history/SID filtering)

After successful execution you will see the following messages:<br/></br>
For SID filtering: <br/></br>
“Setting the trust to not filter SIDs” or “SID filtering is not enabled for this trust”. </br></br>
For SID history: </br></br>
“Enabling SID history for this trust” or “SID history is already enabled for this trust”.

>[!div class="step-by-step"]
[« Step 6](sp1-step6-setup-pam-trust.md)
[Step 8 »](sp1-step8-pam-deployment-verification.md)
