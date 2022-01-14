---

title: Use Azure MFA Server to activate PAM or SSPR Scenarios | Microsoft Docs
description: Set up Azure MFA Server as a second layer of security when your users activate roles in Privileged Access Management and Self Service Password Reset.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 1/5/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

---
# Use Azure MFA Server to activate PAM or SSPR
The following document describes how to setup the Azure MFA Server as a second layer of security when your users activate roles in Privileged Access Management or Self-Service Password Reset.

The article below outlines the configuration update and steps to enable existing deployments of MIM moving from MIM using the Azure MFA SDK to MIM using the Azure MFA Server.

> [!NOTE]
> Microsoft no longer offers Azure MFA Server for new deployments. To use a different MFA provider, see the article on how to [use Custom Multi-Factor Authentication API](Working-with-custommfaserver-for-mim.md).

## Prerequisites

In order to migrate to use Azure MFA Server with MIM, you need:

- Internet access from each MIM Service or MFA Server providing PAM and SSPR, to contact the Azure MFA service
- An Azure subscription
- Install is already using Azure MFA SDK from July 2019 or earlier
- Azure Active Directory Premium licenses for candidate users
- Phone numbers for all candidate users
- MIM hotfix 4.5. or greater see [version history](./reference/version-history.md) for announcements

## Azure MFA Server Configuration
> [!NOTE] 
> In the configuration you will need a valid SSL certificate installed for the SDK. 

### Step 1: Download Azure MFA Server from the Azure portal

> [!IMPORTANT]
> As of July 1, 2019, Microsoft no longer offers MFA Server for new deployments.

Sign-in to the [Azure portal](https://portal.azure.com/) and follow the instructions in [Getting started with MFA Server](/azure/active-directory/authentication/howto-mfaserver-deploy) to download the Azure MFA server.
![Azure portal Server settings](/azure/active-directory/authentication/media/howto-mfaserver-deploy/downloadportal.png)

### Step 2: Generate activation credentials
Use the **Generate activation credentials to initiate use** link to generate activation credentials. Once generated, save for later use.

### Step 3: Install the Azure MFA Server
Once you have downloaded the server, [install](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) it.  Your activation credentials will be required.

### Step 4: Create your IIS Web Application that will host the SDK
1. Open IIS Manager
![IIS Manager](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Create new Website call "MIM MFASDK" , link it to an empty directory 
![Adding a website in IIS Manager](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Open Multi-Factor Authentication Console and click on Web Service SDK
![MFA Server application, button to install Web Service SDK](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Once wizards it click through config, Select "MIM MFASDK" and app pool

> [!NOTE] 
> Wizard will require a admin group to be created. More information can be found on the Azure > > MFA Azure MFA Server documentation.

5. Next we need to import the MIM Service account. Open Multi-Factor Authentication Console and select "Users".

    a. Click on "Import from Active Directory".
    b. Navigate to the service account, such as "contoso\mimservice".
    c. Click "Import" and "Close".

   ![importing users from Azure AD in the MFA console](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG)
6. Edit the MIM Service account to Enable in Multi-Factor Authentication Console.
![Editing a user in the MFA console](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
1. Update the IIS authentication on the "MIM MFASDK" website. First, we will disable the "Anonymous Authentication", then Enable Windows Authentication".
![Changing authentication in IIS Manager](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
1. Final Step: Add the MIM service account to the "PhoneFactor Admins"
![Add a user to an AD group](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## Configuring the MIM Service for Azure MFA Server

### Step 1: Patch Server to 4.5.202.0
 
### Step 2: Backup and Open the MfaSettings.xml located in the "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### Step 3: Update the following lines
1. Remove/Clear the following configuration entries lines <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Update or add the following lines to the following to MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Restart MIM Service and test Functionality with Azure MFA Server.

> [!NOTE] 
> To revert setting replace MfaSettings.xml with your backup file in step 2


## See also

-    [Getting started with the Azure MFA Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [What is Azure MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Use Custom Multi-Factor Authentication API to activate PAM or SSPR](Working-with-custommfaserver-for-mim.md)
- [MIM version release history](./reference/version-history.md)
