---

title: Use Azure Multi-Factor Authentication Server to activate PAM or SSPR Scenarios | Microsoft Docs
description: Set up Azure Multi-Factor Authentication Server as a second layer of security when your users activate roles in Privileged Access Management and Self Service Password Reset.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 1/5/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

---
# Use Azure Multi-Factor Authentication Server to activate PAM or SSPR
The following document describes how to setup the Azure MFA server as a second layer of security when your users activate roles in Privileged Access Management or Self-Service Password Reset.

> [!IMPORTANT]
> Due to the deprecation of Azure Multi-Factor Authentication (MFA) Software Development Kit (SDK), customers will not be able to download the Azure MFA SDK anymore.

The Article below outlines the configuration update and steps to enable moving from Azure MFA SDK to Azure MFA Server.

## Prerequisites

In order to use Azure Multi-Factor Authentication Server with MIM, you need:

- Internet access from each MIM Service or MFA Server providing PAM and SSPR, to contact the Azure MFA service
- An Azure subscription
- Install is already using Azure MFA SDK
- Azure Active Directory Premium licenses for candidate users
- Phone numbers for all candidate users
- MIM hotfix 4.5. or greater see [version history](./reference/version-history.md) for announcements

## Azure Multi-Factor Authentication Server Configuration 
> [!NOTE] 
> In the configuration you will need a valid SSL certificate installed for the SDK. 

### Step 1: Download Azure Multi-Factor Authentication Server from the Azure portal 
Sign-in to the [Azure portal](https://portal.azure.com/) and download the Azure MFA server.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### Step 2: Generate activation credentials
Use the **Generate activation credentials to initiate use** link to generate activation credentials. Once generated, save for later use.

### Step 3: Install the Azure Multi-Factor Authentication Server
Once you have downloaded the server, [install](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) it.  Your activation credentials will be required. 

### Step 4: Create your IIS Web Application that will host the SDK
1. Open IIS Manager
![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Create new Website call "MIM MFASDK" , link it to an empty directory 
![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Open Multi-Factor Authentication Console and click on Web Service SDK
![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Once wizards it click through config, Select "MIM MFASDK" and app pool

> [!NOTE] 
> Wizard will require a admin group to be created. More information can be found on the Azure > > MFA Azure Multi-Factor Authentication Server documentation.

5. Next we need to import the MIM Service account open Multi-Factor Authentication Console select "Users"
    a. Click on "Import from Active Directory"
    b. Navigate to the service account, such as "contoso\mimservice"
    c. Click "Import" and "Close"
   ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Edit the MIM Service account to Enable in Multi-Factor Authentication Console
![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Update the IIS authentication on the "MIM MFASDK" website. First we will disable the "Anonymous Authentication" then Enable Windows Authentication"
![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Final Step: Add the MIM service account to the "PhoneFactor Admins"
![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## Configuring the MIM Service for Azure Multi-Factor Authentication Server 

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

3. Restart MIM Service and test Functionality with Azure Multi-Factor Authentication Server.

> [!NOTE] 
> To revert setting replace MfaSettings.xml with your backup file in step 2


## See also

-    [Getting started with the Azure Multi-Factor Authentication Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [What is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Use Custom Multi-Factor Authentication API to activate PAM or SSPR](Working-with-custommfaserver-for-mim.md)
- [MIM version release history](./reference/version-history.md)
