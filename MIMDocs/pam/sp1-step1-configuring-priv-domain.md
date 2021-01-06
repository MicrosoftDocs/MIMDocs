---
# required metadata

title: Step 1 Configuring the PRIV domain
description: Prepare the PRIV domain with existing or new identities to be managed by Microsoft Identity Manager using scripts
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
# Step 1 Configuring the PRIV domain

> [!div class="step-by-step"]
> [Step 2 »](sp1-step2-configuring-corp-domain.md)

1. Log in to the PRIVDC as Administrator
   * If this PAM deployment is a PRIV-Only environment, login to the CORPDC
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Select Menu Option 1 (PRIV Forest Configuration)


The Service Accounts required for managing SQL, SharePoint and MIM are automatically created, if they are not already present in the domain. You will be prompted to enter the passwords for creation of these service accounts during script execution.

If the PRIV domain is Windows Server 2016, with the Functional Level set to Windows Server 2016, the script will prompt for enabling the optional Active Directory ‘Privileged Access Management Feature’ required by PAM. Confirm ‘Yes’ to proceed. For functional levels below Windows Server 2016, dismiss the warning that additional configuration will not be performed. You will need to rerun the PAMDeployment.ps1 and PAM Forest Configuration, once the administrator raises the functional level to Windows Server 2016.

>[!NOTE]
>The following steps are not required for PRIVOnly configurations

7. Copy the SIDs.txt that is generated in $env:SYSTEMDRIVE\PAM to the similar folder on the CORPDC.
   * You will need this list of SIDs on the CORPDC, to set up permissions in a subsequent step for PRIV users to  be able to read CORP user properties.
8. Once the script completes, it will prompt you to reboot the machine for the changes to take effect.

> [!div class="step-by-step"]
> [Step 2 »](sp1-step2-configuring-corp-domain.md)
