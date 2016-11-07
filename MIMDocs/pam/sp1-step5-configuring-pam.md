---
title: Step 5 Installing/Configuring PAM
description: Prepare the CORP domain with existing or new identities to be managed by Privileged Identity Manager using scripts
keywords:
author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
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
# Step 5 Installing/configuring PAM

>[!div class="step-by-step"]
[« Step 4](sp1-step4-configuring-sharepoint.md)
[Step 6 »](sp1-step6-setup-pam-trust.md)

For a domain joined PAMServer, login as MIMAdmin, otherwise login as a local administrator.
1. Run PowerShell as Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. select menu option 5 (MIM PAM Setup)

>[!NOTE]
>If the machine is not already domain joined to PRIV, it will ask for credentials. After the domain join, the machine will reboot.

After the PAMServer reboots, log back into the machine with the MIMAdmin account.

1. Run PowerShell as Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. select menu option 5 (MIM PAM Setup)

  When prompted, enter the password the MIM Monitor Account, MIM Component Account, MIM MA Account, MIM Service Account, MIM Admin Account and the SharePoint Account.
  Once installation completes, the machine will reboot.

>[!div class="step-by-step"]
[« Step 4](sp1-step4-configuring-sharepoint.md)
[Step 6 »](sp1-step6-setup-pam-trust.md)
