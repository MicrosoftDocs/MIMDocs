---
# required metadata

title: Step 2 Configuring the CORP domain
description: This article describes the second step required to configure the corp domain which involves running a script after sids.txt is copied to the CORPDC
keywords:
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/14/2023
ms.topic: article
ms.service: microsoft-identity-manager

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

> [!div class="step-by-step"]
> [« Step 1](sp1-step1-configuring-priv-domain.md)
> [Step 3 »](sp1-step3-installing-configuring-sql.md)

## Set up the CORP forest

If you are just starting off with PAM, and want to set up a test environment, the script also allows configuration of a CORP Domain. If you already have a CORP domain, continue at the section **Configure the CORP forest** below.

After unzipping the compressed file to the $env:SYSTEMDRIVE\PAM folder, edit the PAMDeploymentConfig.xml adding the details of the CORP forest. Update the DNSName, NetbiosName, DC name, Database/Log Path, and sysvol folder Path. The functional level should be at least Windows Server 2012 R2.

1. Log in to the CORP domain DC as Administrator
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. select menu option 10 (CORP forest setup)

The domain controller will reboot automatically upon completion.

## Configure the CORP forest

Once the SIDs.txt is copied to the CORPDC **not required for PRIVOnly deployments**

1. Login to CORPDC as Administrator
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. select Menu Option 2 (CORP Forest Configuration)

> [!div class="step-by-step"]
> [« Step 1](sp1-step1-configuring-priv-domain.md)
> [Step 3 »](sp1-step3-installing-configuring-sql.md)
