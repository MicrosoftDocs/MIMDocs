---
title: Use Microsoft Entra multifactor authentication to activate PAM
description: Set up Microsoft Entra multifactor authentication as a second layer of security when your users activate roles in Privileged Access Management.
keywords:
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: amycolannino
ms.date: 09/14/2023
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
---

# Using Microsoft Entra multifactor authentication for activation in MIM PAM

> [!IMPORTANT]
> In September 2022, Microsoft announced deprecation of Azure Multi-Factor Authentication Server. Beginning September 30, 2024, Azure Multi-Factor Authentication Server deployments will no longer service multifactor authentication (MFA) requests.  Customers of Azure Multi-Factor Authentication Server should plan to move to instead use either custom MFA providers or Windows Hello or smartcard-based authentication in AD.

When configuring a PAM role, you can choose how to authorize users that request to activate the role. The choices that the PAM authorization activity implements are:

- Role owner approval
- [Microsoft Entra multifactor authentication](/azure/multi-factor-authentication/multi-factor-authentication)

If neither check is enabled, candidate users are automatically activated for their role.

Microsoft Entra multifactor authentication is an authentication service that requires users to verify their sign-in attempts by using a mobile app, phone call, or text message.

> [!NOTE]
> The PAM approach with a bastion environment provided by MIM is intended to be used in a custom architecture for isolated environments where Internet access is not available, where this configuration is required by regulation, or in high impact isolated environments like offline research laboratories and disconnected operational technology or supervisory control and data acquisition environments.  As Microsoft Entra multifactor authentication is an Internet service, this guidance is provided solely for existing MIM PAM customers or those in environments where this configuration is required by regulation. If your Active Directory is part of an Internet-connected environment, see [securing privileged access](/security/compass/overview) on where to start.

## Prerequisites

In order to use Microsoft Entra multifactor authentication with MIM PAM, you need:

- Internet access from each MIM Service providing PAM, to contact the Microsoft Entra multifactor authentication Service
- An Azure subscription
- Azure Multi-Factor Authentication Server from before July 1, 2019
- Microsoft Entra ID P1 or P2 licenses for candidate users
- Phone numbers for all candidate users

<a name='downloading-the-azure-ad-multi-factor-authentication-service-credentials'></a>

## Downloading the Microsoft Entra multifactor authentication Service Credentials

See [Using Azure Multi-Factor Authentication Server in PAM or SSPR](../working-with-mfaserver-for-mim.md) For information on using Azure Multi-Factor Authentication Server.


<a name='configuring-the-mim-service-for-azure-ad-multi-factor-authentication'></a>

## Configuring the MIM Service for Microsoft Entra multifactor authentication

1.  On the computer where the MIM Service is installed, sign in as an administrator or as the user who installed MIM.

2.  Create a new directory folder under the directory where the MIM Service was installed, such as ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Using Windows Explorer, navigate into the ```pf\certs``` folder of the ZIP file downloaded in the previous section. Copy the file ```cert\_key.p12``` to the new directory.

4.  Using Windows Explorer, navigate into the ```pf``` folder of the ZIP, and open the file ```pf\_auth.cs``` in a text editor like Notepad.

5. Find these three parameters: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Copy values from pf\_auth.cs file - screenshot](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Using Notepad, open **MfaSettings.xml** located in ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copy the values from the LICENSE\_KEY, GROUP\_KEY, and CERT\_PASSWORD parameters in the pf\_auth.cs file into their respective xml elements in the MfaSettings.xml file.

8. In the **\<CertFilePath\>** XML element, specify the full path name of the cert\_key.p12 file extracted earlier.

9. In the **\<username\>** element, enter any username.

10. In the **\<DefaultCountryCode\>** element, enter the country code for dialing your users, such as 1 for the United States and Canada. This value is used in case users are registered with telephone numbers that do not have a country code. If a user’s phone number has an international country code distinct from that configured for the organization, then that country code must be included in the phone number that will be registered.

11. Save and overwrite the **MfaSettings.xml** file in the MIM Service folder ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> At the end of the process, ensure that the file **MfaSettings.xml**, or any copies of it or the ZIP file are not publically readable.

<a name='configure-pam-users-for-azure-ad-multi-factor-authentication'></a>

## Configure PAM users for Microsoft Entra multifactor authentication

For a user to activate a role that requires Microsoft Entra multifactor authentication, the user's telephone number must be stored in MIM. There are two ways this attribute is set.

First, the `New-PAMUser` command copies a phone number attribute from the user's directory entry in CORP domain, to the MIM Service database. Note that this is a one-time operation.

Second, the `Set-PAMUser` command updates the phone number attribute in the MIM Service database. For example, the following replaces an existing PAM user's phone number in the MIM Service. Their directory entry is unchanged.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

<a name='configure-pam-roles-for-azure-ad-multi-factor-authentication'></a>

## Configure PAM roles for Microsoft Entra multifactor authentication

Once all of the candidate users for a PAM role have their telephone numbers stored in the MIM Service database, the role can be configured to require Microsoft Entra multifactor authentication. This is done using the `New-PAMRole` or `Set-PAMRole` commands. For example,

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Microsoft Entra multifactor authentication can be disabled for a role by specifying the parameter "-MFAEnabled 0" in the `Set-PAMRole` command.

## Troubleshooting

The following events can be found in the Privileged Access Management event log:

| ID  | Severity | Generated by | Description |
|-----|----------|--------------|-------------|
| 101 | Error       | MIM Service            | User did not complete Microsoft Entra multifactor authentication (e.g., did not answer the phone) |
| 103 | Information | MIM Service            | User completed Microsoft Entra multifactor authentication during activation                       |
| 825 | Warning     | PAM Monitoring Service | Telephone number has been changed                                |

## Next Steps

- [What is Microsoft Entra multifactor authentication?](/azure/multi-factor-authentication/multi-factor-authentication)
