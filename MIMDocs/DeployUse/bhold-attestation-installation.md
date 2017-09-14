---
# required metadata

title: BHOLD attestation installation | Microsoft Docs
description: BHOLD attestation module lets you designate reviewers and perform reviews
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:


---

# BHOLD attestation installation

The BHOLD Attestation module lets you designate reviewers and perform recurring reviews of the relationships between users and per-application permissions and accounts.

## BHOLD Attestation installation requirements

Before installing the BHOLD Attestation module, you must install the BHOLD Core module on the server on which you plan to install the BHOLD Attestation module. For information about installing the BHOLD Core module, see [BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Because the BHOLD Attestation module contacts sends email messages to users, your environment must have a Simple Mail Transfer Protocol (SMTP) email server, such as Microsoft Exchange Server.

>[!IMPORTANT]
If you are installing both BHOLD Reporting and BHOLD Attestation, you must install BHOLD Reporting before installing BHOLD Attestation.

## Before you begin

Before you begin to install the BHOLD Attestation module, you need to be prepared to provide the information that the BHOLD Attestation Setup wizard requires to complete the installation. The following worksheet will help you record that information so you will be ready to supply it when it is needed.

| **Item**                                    | **Description**                                                                                                                                                                                                           | **Value**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** | When selected, specifies that Active Directory Domain Services security will control access to BHOLD Core.                                                                                                                | Select the check box. **Important:** The installation will fail if this check box is not selected.                                                                                                                                                                                                                   |
| **Domain**                                  | Specifies the domain containing the service account that you created when installing BHOLD Core. For more information, see [BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | The domain name is supplied automatically by the wizard. Change the name only if it is incorrect. **Important:** Specify the domain name by using the NetBIOS (short) name, not the fully qualified domain name (FQDN). For example, if the FQDN of the domain is fabrikam.com, specify the domain name as FABRIKAM. |
| **User**                                    | Specifies the logon name of the BHOLD Core service user account.                                                                                                                                                          | Write the user account name here:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifies the password of the service user account.                                                                                                                                                                       | Write the password here: **Important:** Be sure to keep this password in a hidden, secure location.                                                                                                                                                                                                                  |

## BHOLD Attestation installation

To install the BHOLD Attestation module, log on as a member of the Domain Admins group, download the following file and run it as administrator on the server that you intend to install the BHOLD Attestation module on:

- BholdAttestation*\<Version\>*\_Release.msi

Replace *\<Version\>* with the version number of the BHOLD Attestation release that you are installing.

To run the program file as an administrator, right-click the file and then click **Run as administrator**.

## Next steps

- [BHOLD installation guide](bhold-installation-guide.md)
- [BHOLD developer reference](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD version history](../reference/version-bhold-history.md)