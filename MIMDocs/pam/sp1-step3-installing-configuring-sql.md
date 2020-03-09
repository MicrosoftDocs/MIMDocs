---
# required metadata

title: Step 3 Configuring SQL
description: This article is step 3 of the series of articles covering how to configure Privileged Identity Manager using scripts and it discusses the SQL server configuration steps.
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
# Step 3 Configuring SQL

> [!div class="step-by-step"]
> [« Step 2](sp1-step2-configuring-corp-domain.md)
> [Step 4 »](sp1-step4-configuring-sharepoint.md)

Before you move forward with the steps below make sure that you are using SQL Server 2012 SP1 or later, or SQL server 2014. For domain joined servers, login using the MIMAdmin account otherwise login as a local administrator
1. Run PowerShell as Administrator
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Select Menu Option 3 (SQL Server Setup)

   If the server is not yet domain joined to the PRIV domain, it will prompt you for credentials, and join the server to the domain.
   After domain join, the machine will reboot. Upon successful reboot, logon to the server with the MIMAdmin account.

5. Run PowerShell as administrator
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Select Menu Option 3(SQL Server Setup)

When prompted, provide the password for the MIMAdmin service account, and let the installation continue. Once complete, move on to Step 4.

> [!div class="step-by-step"]
> [« Step 2](sp1-step2-configuring-corp-domain.md)
> [Step 4 »](sp1-step4-configuring-sharepoint.md)
