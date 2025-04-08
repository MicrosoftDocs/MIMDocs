---
# required metadata

title: MIM PAM Deployment Scripts
description: This page is part of the series of articles about configuring Microsoft Identity Manager using scripts. It includes a list of the assumptions about the environment.
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
# MIM PAM deployment scripts

MIM 2016 Service Pack 1 introduced a set of deployment scripts to make it easier to deploy PAM. These scripts are available at the download center. Before you attempt to use the scripts, it is important that you make sure that you meet the requirements below:

1. The operating system on all servers is at least Windows Server 2012 R2.
2. DNS must be configured to enable name resolution between your Domain Controllers and component servers.
3. Installation binaries must be available locally on the designated servers for installation of SQL, SharePoint, and MIM.
4. The environment has three dedicated (physical or virtual) machines independently running CORPDC, PRIVDC, and PAMSERVER.
5. For the validation option, you need to have a dedicated workstation.

>[!NOTE]
>If you run into any problem with script execution you may need to take a look at the logs. All script logs are saved in %AppData%\MIMPAMInstall. Please compress the folder into a Zip file and include this, along with details of the operation and the error, in your support case.

Ready to get started with the PAM deployment scripts? Start at [Configure PAM using scripts](./pam/sp1-pam-configure-using-scripts.md).
