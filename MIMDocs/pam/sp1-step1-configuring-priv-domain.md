---
# required metadata

title: Step 1 Configuring the Priv domain
description: Prepare the CORP domain with existing or new identities to be managed by Privileged Identity Manager using scripts
keywords:
author: billmath
ms.author: billmath
manager: mtillman
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
# Step 1 Configuring the Priv domain

> [!div class="step-by-step"]
> [Step 2 »](sp1-step2-configuring-corp-domain.md)

1. Login to the PRIVDC as Administrator
   * If this is a PRIV-Only environment, login to the CORPDC
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Select Menu Option 1 (PRIV Forest Configuration)


The Service Accounts required for managing SQL/SharePoint & MIM are automatically created if they are not already present in the domain. You will be prompted to enter the passwords for creation of these service accounts during script execution.
If the PRIV domain is Windows Server 2016, with the Functional Level set to Windows Server 2016 Technical Preview 5, the script will prompt for enabling the optional Active Directory ‘Privileged Access Management Feature’ required by PAM. Confirm ‘Yes’ to proceed.
For functional levels below Windows Server 2016, dismiss the warning that additional configuration will not be performed. You will need to re-run the PAMDeployment.ps1 and PAM Forest Configuration, once the administrator raises the functional level to Windows Server 2016.

>[!NOTE]
>The following steps are Not required for PRIVOnly configurations

Copy the SIDs.txt that is generated in $env:SYSTEMDRIVE\PAM to the similar folder on the CORPDC. This is required by the CORPDC to setup permissions for PRIV users to read CORP user properties.
Once the script completes, it will prompt you to reboot the machine for the changes to take effect.

> [!div class="step-by-step"]
> [Step 2 »](sp1-step2-configuring-corp-domain.md)
