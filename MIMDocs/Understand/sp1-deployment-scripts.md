---
# required metadata

title: MIM2016 SP1 PAM Deployment Scripts
description: Prepare the CORP domain with existing or new identities to be managed by Privileged Identity Manager using scripts
keywords:
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
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

# MIM2016 SP1 PAM deployment scripts

In this service pack we are introducing a set of deployment scripts to make it easier to deploy PAM. These scripts are available at the download center . Before you attempt to use the scripts it is important that you make sure that the assumptions below apply to your environment.

Important Assumptions:
1. The operating system on all machines is at least Windows Server 2012 R2. If you are trying out Windows Server 2016 Technical Preview 5, the PRIV domain controller must be installed with the TP5 build.
2. DNS must be configured so that name resolution between your Domain Controllers and component servers is automatic.
3. Installation binaries must be available locally on the designated servers for installation of SQL, SharePoint, and MIM.
4. The environment has three dedicated (physical or virtual) machines independently running CORPDC, PRIVDC, and PAMSERVER.
5. For the validation option, it is assumed a dedicated client machine exists to execute this step.

>[!NOTE]
>If you run into any problem with script execution you may need to take a look at the logs. All script logs are saved in %AppData%\MIMPAMInstall. Please compress the folder into a Zip file and email it to mim2016@microsoft.com along with details of the operation and the error.

Ready to get started with the PAM deployment scripts? Start at [Configure PAM using scripts](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts).
