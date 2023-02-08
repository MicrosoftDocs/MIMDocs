---
# required metadata

title: Configure PAM using Scripts
description: This article is part of the series for configuring PAM using scripts. It covers the modification of the XML file that will be used by the PAM deployment scripts.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
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

# Configure PAM using scripts

If you choose to install SQL and SharePoint on separate servers, they must be configured using the instructions below. If SQL, SharePoint and the PAM components are installed on the same machine, the below steps must be run from that machine.

The steps below assume that a PRIV Domain is already setup, for instructions to configure a PRIV domain, view the addendum at the end of the document.

steps:

1. Download [the PAM deployment scripts](https://www.microsoft.com/download/details.aspx?id=53941)
2. Unzip the compressed file “PAMDeploymentScripts.zip” to the %SYSTEMDRIVE%\PAM folder on all machines.
3. On any one of the machines, open the **PAMDeploymentConfig.xml** file and update the details using the chart below or guidance within the XML file itself. If the CORP and PRIV forests are already setup, all you need to update are the **DNSName** and the **NetbiosName.**
4. In the Roles section, update the **service account**, **machine details**, and the **location of the installation binaries** for SQL, SharePoint and MIM roles.
    1. The MIM binary location must point to the directory containing the “Service and Portal” folder. The Client binary location must point to the directory containing the “Add-ins and Extensions.msi”.

5. If this is a PRIVOnly environment, in which there is no CORP forest, then the PRIVOnly tag must be set to True.
    1. For PRIVOnly environments, update the **DNSName** and **NetbiosName** of the PRIV Domain to match the CORP domain. Make sure the machine suffixes are correct for the machines where SQL, SharePoint, and MIM will be installed, as the default template file assumes a CORP and PRIV configuration.

6. Copy the same PAMDeploymentConfig.xml to %SYSTEMDRIVE%\PAM folder on all the machines, CORPDC, PRIVDC, PAM Server, SQL Server, and SharePoint servers.


## Deployment worksheet

Before you proceed update the PAMDeploymentConfig.xml and place the updated copy on all machines.

### Setup

|Machine   | Who to run as   |Commands   |
|---|---|---|
|  PRIVDC |PRIV Domain Admin   | .\PAMDeployment.ps1 Select menu option 1 (PRIV Forest Configuration)   |
|   |   |  The above step generates a SIDs.txt. This file needs to be copied into $envDrive:PAM of the CORPDC before running the next step. |
| CORPDC  |CORP Domain Admin   | .\PAMDeployment.ps1 Select menu option 2 (CORP Forest Configuration)   |
| PAMServer (or SQL Server)   |CORP Domain Admin   |  .\PAMDeployment.ps1 Select menu option 2 (CORP Forest Configuration)  |
|  PAMServer |  Local Admin (MIM Admin after domain join) |  .\PAMDeployment.ps1 Select menu option 4 (SharePoint Setup)  |
| PAMServer  | Local Admin (MIM Admin after domain join)  | .\PAMDeployment.ps1 Select menu option 5 (MIM PAM Setup)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Select menu option 6 (PAM Trust Setup) .\PAMDeployment.ps1 Select menu option 6 (PAM Trust Setup) |

### Validation

|  Machine | Who to run as   | Commands   |
|---|---|---|
| CORPClient  | CORP User (local admin)  |   .\PAMDeployment.ps1 Select menu option 7 (MIM PAM Client Setup)  |
| CORPDC  | CORP Domain Admin   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | CORP User (local admin)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | `<PRIV>\PRIV.pamRequestor` user and in the case of PRIVOnly : `<CORP>\pamrequestor`   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Start »](sp1-step1-configuring-priv-domain.md)
