---
# required metadata

title: Working with the Self-Service Password Reset portal | Microsoft Docs
description: See what's new with Self-Service Password Reset in MIM 2016, including how SSPR works with multi-factor authentication.
keywords:
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
> [!IMPORTANT]
> Due to the announcement of Deprecation of Azure Multi-Factor Authentication Software Development Kit. The Azure MFA SDK will be supported for existing customers up until the retirement date of November 14, 2018. New customers and current customers will be unable to download SDK anymore via the Azure classic portal. To download you will need to reach out to Azure customer support to receive your generated MFA Service Credentials package. <br> The Microsoft development team is working on planning changes to the MFA by integrating with MFA Server SDK.This will be included in upcoming hotfix in early 2018.

# Working with Self-Service Password Reset
Microsoft Identity Manager 2016 provides additional functionality to the Self Service Password Reset feature. This functionality has been enhanced with several important features:

-   The Self-Service Password Reset portal and Windows Log In screen now let users unlock their accounts without changing their passwords or calling support administrators. Users can get locked out of their accounts for many legitimate reasons, like if they enter an old password, use bilingual computers and have the keyboard set to the wrong language, or attempt to log into a shared workstation already open to someone else's account.

-   A new authentication gate,  Phone Gate, was added. This enables user authentication via telephone call.

-   Support has been added for Microsoft Azure Multi-Factor Authentication (MFA) service. This can be used for either the existing SMS One-Time-Password Gate or the new Phone Gate.

## Azure for Multi-Factor Authentication
Microsoft Azure Multi-Factor Authentication is an authentication service that requires users to verify their sign-in attempts with a mobile app, phone call, or text message. It is available to use with Microsoft Azure Active Directory, and as a service for cloud and on-prem enterprise applications.

Azure MFA provides an additional authentication mechanism that can reinforce existing authentication processes, such as the one carried out by MIM for self-service login assistance.

When using Azure MFA, users authenticate with the system in order to verify their identity while trying to regain access to their account and resources. Authentication can be via SMS or via telephone call.   The stronger the authentication, the higher the confidence that the person trying to gain access is indeed the real user who owns the identity. Once authenticated, the user can choose a new password to replace the old one.

## Prerequisites to set up self-service account unlock and password reset using MFA
This section assumes that you have downloaded and completed the deployment of the Microsoft Identity Manager 2016, including the following components and services:

-   A Windows Server 2008 R2 or later has been set up as an Active Directory server including AD Domain Services and Domain Controller with a designated domain (a “corporate” domain)

-   A Group Policy is defined for Account lockout

-   MIM 2016 Synchronization Service (Sync) is installed and running on a server that is domain-joined to the AD domain

-   MIM 2016 Service &amp; Portal including the SSPR Registration Portal and the SSPR Reset Portal, are installed and running on a server (could be co-located with Sync)

-   MIM Sync is configured for AD-MIM identity synchronization, including:

    -   Configuring the Active Directory Management Agent (ADMA) for connectivity to AD DS and capability to import identity data from and export it to Active Directory.

    -   Configuring the MIM Management Agent (MIM MA) for connectivity to FIM Service DB and capability to import identity data from and export it to the FIM database.

    -   Configuring Synchronization Rules in the MIM Portal to allow user data synchronization and facilitate sync-based activities in the MIM Service.

-   MIM 2016 Add-ins &amp; Extensions including the SSPR Windows Login integrated client is deployed on the server or on a separate client computer.

## Prepare MIM to work with multi-factor authentication
Configure MIM Sync to Support Password Reset and Account Unlock Functionality. For more information, see [Installing the FIM Add-ins and Extensions](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Installing FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR Authentication Gates](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) and [the SSPR Test Lab Guide](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

In the next section, you will set up your Azure MFA provider in Microsoft Azure Active Directory. As part of this, you’ll generate a file that includes the authentication material which MFA requires to be able to contact Azure MFA.  In order to proceed, you will need an Azure subscription.

### Register your multi-factor authentication provider in Azure

1.  Create an [MFA provider](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Open a support case and request the direct SDK for ASP.net 2.0 C#. The SDK will only be provided to current users of MIM with MFA because the direct SDK has been deprecated. New customers should adopt the next version of MIM that will integrate with MFA server.

3.  Click **App Services &gt; Active Directory &gt; Multi-Factor Auth Provider &gt; Quick Create**.

![Azure Portals quick create MFA image](media/MIM-SSPR-Azureportal.png)

4.  In the **Name** field enter **SSPRMFA**, then click **Create**.

![Azure Portal MFA image](media/MIM-SSPR-Azureportal-2.png)

5.  Click **Active Directory** in the Azure classic portal menu, and then click the **Multi-Factor Auth Providers** tab.

6.  Click on **SSPRMFA** and then click **Manage** at the bottom of the screen.

    ![Azure portal Manage icon](media/MIM-SSPR-ManageButton.png)

7.  In the new window, on the left panel under **Configure**, click on **Settings**.

8.  Under **Fraud Alert**, uncheck **Block user when fraud is reported. This is done in order to prevent blocking the entire service.

9. In the **Azure Multi-Factor Authentication** window that opens, click **SDK** under **Downloads** in the menu at the left.

10. Click the **Download** link in the ZIP column for the file with language **SDK for ASP.net 2.0 C#**.

    ![Azure MFA download zip file image](media/MIM-SSPR-Azure-MFA.png)

11. Copy the resulting ZIP file to each system where MIM Service is installed.  Please be aware that the ZIP file contains keying material which is used to authenticate to the Azure MFA service.

### Update the configuration file

1. Sign into the computer where MIM Service is installed, as the user who installed MIM.

2. Create a new directory folder located below the directory where the MIM Service was installed, such as **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Using Windows Explorer, navigate into the **\pf\certs** folder of the ZIP file downloaded in the previous section, and copy the file **cert_key.p12** to the new directory.

4.  In the SDK zip file, in the folder **\pf**, open the file **pf_auth.cs**.

5.  Find these three parameters: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![pf_auth.cs code image](media/MIM-SSPR-pFile.png)

6.  In **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**, open the file: **MfaSettings**.xml.

7.  Copy the values from the `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` parameters in the pf_aut.cs file into their respective xml elements in the MfaSettings.xml file.

8.  In the SDK zip file, under \pf\certs, extract the file **cert_key.p12** and enter the full path to it in the MfaSettings.xml file into the `<CertFilePath>` xml element.

9. In the `<username>` element enter any username.

10. In the `<DefaultCountryCode>` element enter your default country code. In case phone-numbers are registered for users without a country code, this is the country code they will get. In case a user has an international country code, it has to be included in the registered phone number.

11. Save the MfaSettings.xml file with the same name in the same location.

#### Configure the Phone gate or the One-Time Password SMS Gate

1.  Launch Internet Explorer and navigate to the MIM Portal, authenticating as the MIM administrator, then click on  **Workflows** in the left hand navigation bar.

    ![MIM Portal navigation image](media/MIM-SSPR-workflow.jpg)

2.  Check **Password Reset AuthN Workflow**.

    ![MIM Portal Workflows image](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Click on the **Activities** tab and then scroll down to **Add Activity**.

4.  Select **Phone Gate** or  **One-Time Password SMS Gate** click **Select** and then **OK**.

Users in your organization can now register for password reset.  During this process, they will enter their work phone number or mobile phone number so the system knows how to call them (or send them SMS messages).

#### Register users for password reset

1.  A user will launch a web browser and navigate to the MIM Password Reset Registration Portal.  (Typically this portal will be configured with Windows authentication).  Within the portal, they will provide their username and password again to confirm their identity.

    They need to enter the Password Registration Portal and authenticate using their username and password.

2.  In the **Phone Number** or **Mobile Phone**  field, they have to enter a country code, a space, and the phone number and click **Next**.

    ![MIM Phone Verification image](media/MIM-SSPR-PhoneVerification.JPG)

    ![MIM Mobile Phone Verification image](media/MIM-SSPR-mobilephoneverification.JPG)

## How does it work for your users?
Now that everything is configured and it’s running, you might want to know what your users are going to have to go through when they reset their passwords right before a vacation and come back only to realize that they completely forgot their passwords.

There are two ways a user can use the password reset and account unlock functionality, either from the Windows sign-in screen, or from the self-service portal.

By installing the MIM Add-ins and Extensions on a domain joined computer connected over your organizational network to the MIM Service, users can recover from a forgotten password at the desktop login experience.  The following steps will walk you through the process.

#### Windows desktop login integrated password reset

1.  If your user enters the wrong password several times, in the sign-in screen, they will have the option to click **Problems logging in?** .

    ![Sign-in screen image](media/MIM-SSPR-problemsloggingin.JPG)

    Clicking this link will take them to the MIM Password Reset screen where they can change their password or unlock their account.

    ![MIM Password Reset image](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  The user will be directed to authenticate. If MFA was configured, the user will receive a phone call.

3.  In the background, what’s happening is that Azure MFA then places a phone call to the number the user gave when they signed up for the service.

4.  When a user answers the phone, they will be asked to press the pound key # on the phone. Then the user clicks **Next** in the portal.

    If you set up other gates as well, the user will be asked to provide more information in subsequent screens.

    > [!NOTE]
    > If the user is impatient and clicks **Next** before pressing the pound key #, authentication fails.

5.  After successful authentication, the user will be given two options, either unlock the account and keep the current password or to set a new password.

6.  Then the user has to enter a new password twice, and the password is reset.

#### Access from the self-service portal

1.  Users can open a web browser, navigate to the **Password Reset Portal** and enter their username and click **Next**.

    If MFA was configured, the user will receive a phone call. In the background, what’s happening is that Azure MFA then places a phone call to the number the user gave when they signed up for the service.

    When a user answers the phone, he will be asked to press the pound key # on the phone. Then the user clicks **Next** in the portal.

2.  If you set up other gates as well, the user will be asked to provide more information in subsequent screens.

    > [!NOTE]
    > If the user is impatient and clicks **Next** before pressing the pound key #, authentication fails.

3.  The user will have to choose if he wants to reset his password or unlock his account. If he chooses to unlock his account, the account will be unlocked.

    ![MIM Login Assistant Account Unlock image](media/MIM-SSPR-accountUnlock.JPG)

4.  After successful authentication, the user will be given two options, either to keep his current password or to set a new password.

5.  ![MIM account unlocked success image](media/MIM-SSPR-account-unlock.JPG)

6.  If the user chooses to reset their password, they will have to type in a new password twice and click **Next** to change the password.

    ![MIM Login Assistant Password Reset image](media/MIM-SSPR-PR1.JPG)
