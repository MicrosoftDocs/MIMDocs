---
title: BHOLD reporting Installation | Microsoft Docs
description: BHOLD reporting module allows you to generate reports about roles and authorization policies
author: henrymbuguakiarie
ms.author: henrymbugua
ms.date: 04/08/2025
ms.topic: article
ms.service: microsoft-identity-manager
---


# BHOLD reporting Installation

The BHOLD Reporting module gives you the ability to generate reports about roles and other authorization policies in BHOLD. These reports are often useful for auditing or for demonstrating compliance to regulatory requirements. Also, this module extends your ability to manage authorization within your organization by giving users the information they need to analyze the membership of their roles. The reports can have restricted views that ensure that the users who create reports are shown only the information they are allowed to see.

## BHOLD Reporting installation requirements

Before installing the BHOLD Reporting module, you must install the BHOLD Core module on the server on which you plan to install the BHOLD Reporting module. For information about installing the BHOLD Core module, see [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> If you are installing both BHOLD Reporting and BHOLD Attestation, you must install BHOLD Reporting before installing BHOLD Attestation.

## Before you begin

Before you begin to install the BHOLD Reporting module, you need to be prepared to provide the information that the BHOLD Reporting Setup wizard requires to complete the installation. The following worksheet will help you record that information so you will be ready to supply it when it is needed.

| **Item**                                    | **Description**                                                                                                                                                                                                           | **Value**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** | When selected, specifies that Active Directory Domain Services security will control access to BHOLD Core.                                                                                                                | Select the check box. </br>**Important:** The installation will fail if this check box is not selected.                                                                                                                                                                                                                   |
| **Domain**                                  | Specifies the domain containing the service account that you created when installing BHOLD Core. For more information, see [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | The domain name is supplied automatically by the wizard. Change the name only if it is incorrect. **Important:** Specify the domain name by using the NetBIOS (short) name, not the fully qualified domain name (FQDN). For example, if the FQDN of the domain is fabrikam.com, specify the domain name as FABRIKAM. |
| **User**                                    | Specifies the logon name of the BHOLD Core service user account.                                                                                                                                                          | Write the user account name here:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifies the password of the service user account.                                                                                                                                                                       | Write the password here: </br>**Important:** Be sure to keep this password in a hidden, secure location.                                                                                                                                                                                                                  |

## BHOLD Reporting installation

To install the BHOLD Reporting module, log on as a member of the Domain Admins group, download the following file and run it as administrator on the server that you intend to install the BHOLD Reporting module on:

- BholdReporting<em>\<Version\></em>\_Release.msi

Replace *\<Version\>* with the version number of the BHOLD Reporting release that you are installing.

To run the program file as an administrator, right-click the file and then click **Run as administrator**.

## Next steps

- [BHOLD installation guide](bhold-installation-guide.md)
- [BHOLD developer reference](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD version history](../reference/version-bhold-history.md)
