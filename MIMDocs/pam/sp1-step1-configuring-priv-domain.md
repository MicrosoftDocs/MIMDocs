---
# required metadata

title: Step 1 Configuring the PRIV domain
description: Prepare the PRIV domain with existing or new identities to be managed by Microsoft Identity Manager using scripts
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/27/2023
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


## Set up the PRIV domain
After unzipping the compressed file into the $env:SYSTEMDRIVE\PAM folder, edit the PAMDeploymentConfig.xml to provide details of the PRIV forest. Update the DNSName, the NetbiosName, the DC name, the Database/Log Path & sysvol folder Path. Also update the Domain & ForestMode to Windows Server 2016 (WinThreshold).

1. Log in to the PRIV domain DC as Administrator
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. select menu option 9 (PRIV Forest setup)


The DC will reboot automatically after completion. The directory Services Restore Mode (DSRM) administrator password must match the following criteria:

  * Password length is a minimum of 15 characters
  * Password contains at least one lowercase character
  * Password contains at least one UPPERCASE character
  * Password contains at least one digit or special character

## Configure the PRIV domain

1. Log in to the PRIVDC as Administrator
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Select Menu Option 1 (PRIV Forest Configuration)


The Service Accounts required for managing SQL, SharePoint and MIM are automatically created, if they are not already present in the domain. You will be prompted to enter the passwords for creation of these service accounts during script execution.

When deploying, the PRIV domain Functional Level should be set to Windows Server 2016. The script will prompt for enabling the optional Active Directory ‘Privileged Access Management Feature’ required by PAM. Confirm ‘Yes’ to proceed. Do not deploy PAM for functional levels below Windows Server 2016, as you will need to rerun the PAMDeployment.ps1 and PAM Forest Configuration, once the administrator raises the functional level to Windows Server 2016.

>[!NOTE]
>The following steps are not required for PRIVOnly configurations

7. Copy the SIDs.txt that is generated in $env:SYSTEMDRIVE\PAM to the similar folder on the CORPDC.
   * You will need this list of SIDs on the CORPDC, to set up permissions in a subsequent step for PRIV users to  be able to read CORP user properties.
8. Once the script completes, it will prompt you to reboot the machine for the changes to take effect.

> [!div class="step-by-step"]
> [Step 2 »](sp1-step2-configuring-corp-domain.md)
