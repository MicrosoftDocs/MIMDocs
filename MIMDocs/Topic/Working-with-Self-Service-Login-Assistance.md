---
title: Working with Self-Service Login Assistance
ms.custom: 
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
author: Kgremban
---
# Working with Self-Service Login Assistance
Microsoft Identity Manager 2016 provides additional functionality to the Self Service Password Reset feature. This functionality has been enhanced with several important features:

-   The Self-Service Password Reset portal and Windows Log In screen now enable uses to unlock their accounts without changing their passwords, in addition to being able to reset their passwords. Commonly, users get locked out of their accounts for many legitimate reasons, such as because they accidentally entered an old password, they use bilingual computers and have the keyboard set to the wrong language or they attempt to log into a shared workstation already open to someone else's account.  Self-Service Account Unlock allows users to unlock their accounts on their own, without necessitating expensive calls to support administrators.

-   A new authentication gate,  Phone Gate, was added. This enables user authentication via telephone call.

-   Support has been added for Microsoft Azure Multi-Factor Authentication (MFA) service. This can be used for either the exiting SMS One-Time-Password Gate or the new Phone Gate.

## Azure for Multi-factor Authentication
When using Azure multi-factor authentication, users authenticate with the system in order to verify their identity while trying to regain access to their account and resources. Authentication can be via SMS or via telephone call.   The stronger the authentication, the higher the confidence that the user trying to gain access is indeed the real user who own the identity. Once authenticated, the user can choose a new password to replace the old one.

Microsoft Azure Multi-Factor Authentication (MFA) is an authentication service that requires users to verify their sign-in attempts by using a mobile app, phone call, or text message. It is available to use with Microsoft Azure Active Directory, and as a service for cloud and on-prem enterprise applications.
Azure MFA provides an additional authentication mechanism that can be integrated into and reinforce existing authentication processes, such as the one carried out by MIM for self-service login assistance.

## Preparing Microsoft Identity Manager to work with the self-service account unlock and password reset using MFA
This section assumes that you have downloaded and completed the deployment of the Microsoft Identity Manager 2016, including the following components and services:

-   A Windows Server 2008 R2 or later has been set up as an Active Directory server including AD Domain Services and Domain Controller with a designated domain (a “corporate” domain).

-   A Group Policy is defined for Account lockout

-   MIM 2016 Synchronization Service (Sync) has to be installed and running on a server that is domain-joined to the AD domain

-   MIM 2016 Service &amp; Portal including the SSPR Registration Portal and the SSPR Reset Portal, have to be installed and running on a server (could be collocated with Sync)

-   MIM Sync has to be configured for AD-FIM identity synchronization, including:

    -   Configuring the Active Directory Management Agent (ADMA) for connectivity to AD DS and capability to import identity data from and export it to Active Directory.

    -   Configuring the MIM Management Agent (FIM MA) for connectivity to FIM Service DB and capability to import identity data from and export it to the FIM database.

    -   Configuring Synchronization Rules in the MIM Portal to allow user data synchronization and facilitate sync-based activities in the MIM Service.

-   MIM 2016 Add-ins &amp; Extensions including the SSPR Windows Login integrated client has to be deployed on the server or on a separate client computer.

## Prerequisites
Configure MIM Sync to Support Password Reset and Account Unlock Functionality. For more information, see [Installing the FIM Add-ins nd Extensions](https://technet.microsoft.com/en-us/library/ff512688%28v=ws.10%29.aspx), [Installing FIM SSPR](https://technet.microsoft.com/en-us/library/hh322891%28v=ws.10%29.aspx), [SSPR Authentication Gates](https://technet.microsoft.com/en-us/library/jj134288%28v=ws.10%29.aspx) and [the SSPR Test Lab Guide](https://technet.microsoft.com/en-us/library/hh826057%28v=ws.10%29.aspx)

In the next section, you will set up your Azure MFA provider in Microsoft Azure Active Directory. As part of this, you’ll generate a file that includes the authentication material which MFA requires to be able to contact Azure MFA.  In order to proceed, you will need an Azure subscription.

#### Register your multifactor authentication provider in Azure

1.  Open a web browser and connect to the Azure management portal at manage.windowsazure.com as a Azure subscription administrator user.

    Log into the Azure portal using your credentials.

2.  In the bottom left hand corner, click **New**.

3.  Click **App Services &gt; Active Directory &gt; Multi-Factor Auth Provider &gt; Quick Create**.

4.  In the **Name** field, enter **SSPRMFA** and click **Create**.

    ![](../Image/MIM_SSPR_Azureportal.png)

5.  Click **Active Directory** in the Azure Portal menu, and then click the **Multi-Factor Auth Providers** tab.

6.  Click on **SSPRMFA** and then click **Manage** at the bottom of the screen.

    ![](../Image/MIM_SSPR_ManageButton.png)

7.  In the new window, on the left panel, under **Configure**, click on **Settings**.

8.  Under **Fraud Alert**, uncheck **Block user when fraud is reported** . This is done in order to prevent blocking the entire service.

9. In the **Azure Multi-Factor Authentication** window that opens, click **SDK** under **Downloads** in the menu at the left.

10. Click the **Download** link in the ZIP column for the file with language **SDK for ASP.net 2.0 C#**.

    ![](../Image/MIM_SSPR_Azure_MFA.png)

11. Copy the resulting ZIP file to each system where MIM Service is installed.  Please be aware that the ZIP file contains keying material which is used to authenticate to the Azure MFA service.

#### Update the configuration file

1.  Log intot he computer where MIM Service is installed, as the user who installed MIM.

2.  Create a new directory folder located below the directory where the MIM Service was installed, such as **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3.  Using Windows Explorer, navigate into the “pf\certs” folder of the ZIP file downloaded in the previous section, and copy the file **cert_key.p12** to the new directory.

4.  In the SDK zip file, in the folder \pf, open the file **pf_auth.cs**.

5.  Find these three parameters: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![](../Image/MIM_SSPR_pFile.png)

6.  In **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**, open the file: **MfaSettings**.xml.

7.  Copy the values from the `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` parameters in the pf_aut.cs file into their respective xml elements in the MfaSettings.xml file.

8.  In the SDK zip file, under \pf\certs, extract the file **cert_key.p12** and enter the full path to it in the MfaSettings.xml file into the `<CertFilePath>` xml element.

9. In the `<username>` element enter any username.

10. In the `<DefaultCountryCode>` element enter your default country code. In case phone-numbers are registered for users without a country code, this is the country code they will get. In case a user has an international country code, it has to be included in the registered phone number.

11. Save the MfaSettings.xml file with the same name in the same location.

#### Configure the Phone gate or the One-Time Password SMS Gate

1.  Launch Internet Explorer and navigate to the MIM Portal, authenticating as the MIM administrator, then click on  **Workflows** in the left hand navigation bar.

    ![](../Image/MIM_SSPR_workflow.jpg)

2.  Check **Password Reset AuthN Workflow**.

    ![](../Image/MIM_SSPR_PwdResetAuthNworkflow.jpg)

3.  Click on the **Activities** tab and then scroll down to **Add Activity**.

4.  Select **Phone Gate** or  **One-Time Password SMS Gate** click **Select** and then **OK**.

Users in your organization can now register for password reset.  During this process, they will enter their work phone number or mobile phone number so the system knows how to call them (or send them SMS messages).

#### Register users for password reset

1.  A user will launch a web browser a navigate to the MIM Password Reset Registration Portal.  (Typically this portal will be configured with Windows authentication).  Within the portal, they will provide their username and password again to confirm their identity.

    They need to enter the Password Registration Portal and authenticate using their username and password.

2.  In the **Phone Number** or **Mobile Phone**  field, they have to enter a country code, a space, and the phone number and click **Next**.

    ![](../Image/MIM_SSPR_PhoneVerification.JPG)

    ![](../Image/MIM_SSPR_mobilephoneverification.JPG)

## How does it work for your users?
Now that everything is configured and it’s running, you might want to know what your users are going to have to go through when they reset their passwords right before a vacation and come back only to realize that they completely forgot their passwords.

There are two ways a user can use the password reset and account unlock functionality, either from the Windows Log in screen, or from the self-service portal.

By installing the MIM Add-ins and Extensions on a domain joined computer connected over your organizational network to the MIM Service, users can recover from a forgotten password at the desktop login experience.  The following steps will walk you through the process.

#### Windows desktop login integrated password reset

1.  If your user enters the wrong password several times, in the login screen, they will have the option to click **Problems logging in?** . ![](../Image/MIM_SSPR_problemsloggingin.JPG)

    Clicking this link will take them to the MIM Password Reset screen where they can change their password or unlock their account.![](../Image/MIM_SSPR_keepcurrentorsetnewpwd.JPG)

2.  The user will be directed to authenticate. If MFA was configured, the user will receive a phone call.

3.  In the background, what’s happening is that Azure MFA then places a phone call to the number the user gave when he signed up for the service.

4.  When a user answers the phone, they will be asked to press the pound key # on the phone. Then the user clicks **Next** in the portal.

    If you set up other gates as well, the user will be asked to provide more information in subsequent screens.

    > [!NOTE]
    > If the user is impatient and clicks **Next** before pressing the pound key #, authentication fails.

5.  After successful authentication, the user will be given two options, either unlock the account and keep the current password or to set a new password.

6.  Then the user has to enter a new password twice, and the password is reset.

#### Access from the self-service portal

1.  Users can open a web browser, navigate to the **Password Reset Portal** and enter their username and click **Next**.

    If MFA was configured, the user will receive a phone call. In the background, what’s happening is that Azure MFA then places a phone call to the number the user gave when he signed up for the service.

    When a user answers the phone, he will be asked to press the pound key # on the phone. Then the user clicks **Next** in the portal.

2.  If you set up other gates as well, the user will be asked to provide more information in subsequent screens.

    > [!NOTE]
    > If the user is impatient and clicks **Next** before pressing the pound key #, authentication fails.

3.  The user will have to choose if he wants to reset his password or unlock his account. If he chooses to unlock his account, the account will be unlocked.

    ![](../Image/MIM_SSPR_accountUnlock.JPG)

4.  After successful authentication, the user will be given two options, either to keep his current password or to set a new password.

5.  ![](../Image/MIM_SSPR-account-unlock.JPG)

6.  If the user chooses to reset their password, they will have to type in a new password twice and click **Next** to change the password.

    ![](../Image/MIM_SSPR_PR1.JPG)

