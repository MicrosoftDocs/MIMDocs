---
title: Addendum
description: This is the Addendum to the documents covering the scripted deployment of PAM. It covers configuring the PRIV and CORP domains as well as a setting up a client to do the validation and information for how to request assistance.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
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
# PAM deployment scripts addendum:

## Addendum 1 Setting up the PRIV domain

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

## Addendum 2 Setting up the CORP domain

If you are just starting off with PAM, and want to set up a test environment, the script also allows configuration of a CORP Domain. After unzipping the compressed file to the $env:SYSTEMDRIVE\PAM folder, edit the PAMDeploymentConfig.xml adding the details of the CORP forest. Update the DNSName, NetbiosName, DC name, Database/Log Path, and sysvol folder Path. The functional level should be at least Windows Server 2012 R2.

1. Log in to the CORP domain DC as Administrator
2. Run PowerShell as Administrator
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. select menu option 10 (CORP forest setup)

The domain controller will reboot automatically upon completion

## Addendum 3 Setting up a CORP client to do the validation

The ClientBinaryLocation in the config file must point to the location where setup.exe is located.
Log in to the client as a local administrator and run the following commands in an elevated PowerShell window:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Select Menu Option 7 (MIM PAM Client Setup)


If the machine is not domain joined, it will prompt for CORP admin credentials to perform domain join. The machine must be rebooted after domain join. Log in to the client again as local administrator and run the following commands from an elevated PowerShell window:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Select Menu Option 7 (MIM PAM Client Setup)

Proceed with Step 8 provided above.

## Addendum 4 if something goes wrong

All script logs are saved in %AppData%\MIMPAMInstall. If needed by support, compress the folder into a Zip file along with details of the operation and the error.
