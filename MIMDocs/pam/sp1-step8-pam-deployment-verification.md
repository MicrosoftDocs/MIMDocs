---
title: Step 8 PAM deployment verification
description: Prepare the CORP domain with existing or new identities to be managed by Privileged Identity Manager using scripts
keywords:
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
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

# Step 8 Pam deployment verification

The Deployment package comes with verification scripts that can execute a PAM scenario to validate the PAM deployment is working as expected.
To use the Deployment Verification, modify the PAMDeploymentConfig.xml section called <PamValidation/> .

>[!Note] Validation requires a Client machine domain joined to the CORP Domain with the PAM client side components installed. Please see Addendum for scripts on how to install a client.

The client machine name must be updated in the <PAMValidationClient/> tag of the PAMDeploymentConfig.xml
The rest of the data in the <PAMValidation/> node needs to be edited only if it conflicts with existing users/groups, as this validation will attempt to create them.
Use the following steps to perform validation:

Step 1:
  * Login to the CORPDC as a CORP Domain Admin
  * Run PowerShell as Administrator
  * cd $env:SYSTEMDRIVE\PAM
  * Import-module .\PAMValidation.psm1
  * Create-PAMValidationonCORPDCConfig

This will create the necessary groups and users needed for validation

Step 2:
  * Login to the PAM Server ad MIMAdmin
  * Run PowerShell as Administrator
  * cd $env:SYSTEMDRIVE\PAM
  * import-module .\PAMValidation.psm1
  * move-PAMVAlidationUsersToPAM

This step migrates the users and groups to the PAM environment

Step 3:
  * Login to the CORP client as a local administrator
  * Run PowerShell as Administrator
  * cd $env:SYSTEMDRIVE\PAM
  * import-module .\PAMValidation.psm1
  * Enable-PAMUsersCORPClientRemote


This step will prompt you for the CORPAdmin credential. Once provided, it will add the required users to the ‘Remote Desktop Users’ and ‘Remote Management Users’ groups.
On the CORP Client, use the following command to open PowerShell as the PRIV user you are validating. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
From the PowerShell window, type:

  * cd $env:SYSTEMDRIVE\PAM
  * import-module .\PAMValidation.psm1
  * test-PAMValidationScenarioNoApprovalRequest


  This will show the status of the request.
  Initially the user will not have access to the resource. After the user is Just-In-Time added to the role, the user is granted access. Once the request duration expires, the user again will not have access.
  The script uses the default (11 minutes) for the request to expire.
