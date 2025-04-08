---
title: Step 4 Configuring SharePoint
description: This is step 4 of configuring PAM with scripts. In this step you configure SharePoint so that it can be used as part of your PAM deployment.
keywords:
author: billmath
ms.author: billmath
manager: femila
ms.date: 04/08/2025
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

# Step 4 Configuring SharePoint

> [!div class="step-by-step"]
> [« Step 3](sp1-step3-installing-configuring-sql.md)
> [Step 5 »](sp1-step5-configuring-pam.md)

When using the script, SharePoint must be SharePoint Foundation 2013 with SP1.  If you wish to deploy MIM Portal on a later version of SharePoint, instead follow the steps for [Preparing Sharepoint](/microsoft-identity-manager/prepare-server-sharepoint) to install SharePoint Server 2016 or 2019.


For domain joined servers, login as MIMAdmin

1. Run PowerShell as administrator
2.  .\PAMDeployment.ps1
3.  Select Menu Option 4 (SharePoint Setup)


For workgroup servers

1. Run PowerShell as administrator
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. select Menu option 4 (SharePoint Setup)

The machine will reboot several times as it installs SharePoint. Each time, the SharePoint setup must be re-run, being sure to login with the MIMAdmin account.
If the machine installing SharePoint does not have Internet connectivity to download pre-requisites, they can be downloaded independently and put in a local folder. This local folder path needs to be updated in the PAMConfiguration.xml file in `<PrerequisitesBinaryLocation/>`.
After installation, the SharePoint Configuration GUI will open, and walk through the steps to complete the SharePoint installation. Please select Complete Server and walk through the rest of the UI. After installation, it will prompt for running the Configuration Wizard. Complete the steps as given below.

1. On the **Connect to server farm** tab, change to **create a new server farm.**
2. Specify the **SQLServer** as the database server for the configuration database, and the **SharePoint ServiceAccount** as the database access account for SharePoint to use.
3. Specify a password as the farm security passphrase **(it will not be used later)**
4. Accept the rest of the SharePoint configuration wizard default settings to make a single-server farm

Details can be found in the **Configure SharePoint** section in [Step 3 - Prepare a PAM server](/microsoft-identity-manager/pam/step-3-prepare-pam-server)
When it completes, run the “.\PAMDeployment.ps1” script again, selecting Option 4 (SharePoint setup) to complete this step.

> [!div class="step-by-step"]
> [« Step 3](sp1-step3-installing-configuring-sql.md)
> [Step 5 »](sp1-step5-configuring-pam.md)
