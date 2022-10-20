---
title: Use Azure AD Multi-Factor Authentication to activate PAM | Microsoft Docs
description: Set up Azure AD Multi-Factor Authentication as a second layer of security when your users activate roles in Privileged Access Management.
keywords:
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
---

# Using Azure AD Multi-Factor Authentication for activation in MIM PAM

> [!IMPORTANT]
> In September 2022, Microsoft announced deprecation of Azure AD Multi-Factor Authentication Server. Beginning September 30, 2024, Azure AD Multi-Factor Authentication Server deployments will no longer service multifactor authentication (MFA) requests.  Customers of Azure AD Multi-Factor Authentication Server should plan to move to instead use either custom MFA providers or Windows Hello or smartcard-based authentication in AD.


When configuring a PAM role, you can choose how to authorize users that request to activate the role. The choices that the PAM authorization activity implements are:

- Role owner approval
- [Azure AD Multi-Factor Authentication](/azure/multi-factor-authentication/multi-factor-authentication)

If neither check is enabled, candidate users are automatically activated for their role.

Microsoft Azure AD Multi-Factor Authentication is an authentication service that requires users to verify their sign-in attempts by using a mobile app, phone call, or text message.

> [!NOTE]
> The PAM approach with a bastion environment provided by MIM is intended to be used in a custom architecture for isolated environments where Internet access is not available, where this configuration is required by regulation, or in high impact isolated environments like offline research laboratories and disconnected operational technology or supervisory control and data acquisition environments.  As Azure AD Multi-Factor Authentication is an Internet service, this guidance is provided solely for existing MIM PAM customers or those in environments where this configuration is required by regulation. If your Active Directory is part of an Internet-connected environment, see [securing privileged access](/security/compass/overview) for more information on where to start.

## Prerequisites

In order to use Azure AD Multi-Factor Authentication with MIM PAM, you need:

- Internet access from each MIM Service providing PAM, to contact the Azure AD Multi-Factor Authentication Service
- An Azure subscription
- Azure AD Multi-Factor Authentication Server from before July 1, 2019
- Azure Active Directory Premium licenses for candidate users
- Phone numbers for all candidate users

## Downloading the Azure AD Multi-Factor Authentication Service Credentials

See [Using Azure AD Multi-Factor Authentication Server in PAM or SSPR](../working-with-mfaserver-for-mim.md) For information on using Azure AD Multi-Factor Authentication Server.


## Configuring the MIM Service for Azure AD Multi-Factor Authentication

1.  On the computer where the MIM Service is installed, sign in as an administrator or as the user who installed MIM.

2.  Create a new directory folder under the directory where the MIM Service was installed, such as ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Using Windows Explorer, navigate into the ```pf\certs``` folder of the ZIP file downloaded in the previous section. Copy the file ```cert\_key.p12``` to the new directory.

4.  Using Windows Explorer, navigate into the ```pf``` folder of the ZIP, and open the file ```pf\_auth.cs``` in a text editor like Notepad.

5. Find these three parameters: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Copy values from pf\_auth.cs file - screenshot](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Using Notepad, open **MfaSettings.xml** located in ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copy the values from the LICENSE\_KEY, GROUP\_KEY, and CERT\_PASSWORD parameters in the pf\_auth.cs file into their respective xml elements in the MfaSettings.xml file.

8. In the **\<CertFilePath\>** XML element, specify the full path name of the cert\_key.p12 file extracted earlier.

9. In the **\<username\>** element enter any username.

10. In the **\<DefaultCountryCode\>** element enter the country code for dialing your users, such as 1 for the United States and Canada. This value is used in case users are registered with telephone numbers that do not have a country code. If a user’s phone number has an international country code distinct from that configured for the organization, then that country code must be included in the phone number that will be registered.

11. Save and overwrite the **MfaSettings.xml** file in the MIM Service folder ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> At the end of the process, ensure that the file **MfaSettings.xml**, or any copies of it or the ZIP file are not publically readable.

## Configure PAM users for Azure AD Multi-Factor Authentication

For a user to activate a role that requires Azure AD Multi-Factor Authentication, the user's telephone number must be stored in MIM. There are two ways this attribute is set.

First, the `New-PAMUser` command copies a phone number attribute from the user's directory entry in CORP domain, to the MIM Service database. Note that this is a one-time operation.

Second, the `Set-PAMUser` command updates the phone number attribute in the MIM Service database. For example, the following replaces an existing PAM user's phone number in the MIM Service. Their directory entry is unchanged.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## Configure PAM roles for Azure AD Multi-Factor Authentication

Once all of the candidate users for a PAM role have their telephone numbers stored in the MIM Service database, the role can be configured to require Azure AD Multi-Factor Authentication. This is done using the `New-PAMRole` or `Set-PAMRole` commands. For example,

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Azure AD Multi-Factor Authentication can be disabled for a role by specifying the parameter "-MFAEnabled 0" in the `Set-PAMRole` command.

## Troubleshooting

The following events can be found in the Privileged Access Management event log:

| ID  | Severity | Generated by | Description |
|-----|----------|--------------|-------------|
| 101 | Error       | MIM Service            | User did not complete Azure AD Multi-Factor Authentication (e.g., did not answer the phone) |
| 103 | Information | MIM Service            | User completed Azure AD Multi-Factor Authentication during activation                       |
| 825 | Warning     | PAM Monitoring Service | Telephone number has been changed                                |

## Next Steps

- [What is Azure AD Multi-Factor Authentication](/azure/multi-factor-authentication/multi-factor-authentication)
