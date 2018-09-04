---

title: Use Azure Multi-Factor Authentication Server SDK to activate PAM or SSPR Scenarios | Microsoft Docs
description: Set up Azure Multi-Factor Authentication Server SDK as a second layer of security when your users activate roles in Privileged Access Management and Self Service Password Reset.
keywords:
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

---
# Use Azure Multi-Factor Authentication Server to activate PAM or SSPR
The following document describes how to setup the Azure MFA server as a second layer of security when your users activate roles in Priviledge Access Management or Self-Service Password Reset.

> [!IMPORTANT]
> Due to the announcement of Deprecation of Azure Multi-Factor Authentication Software Development Kit. The Azure MFA SDK will be supported for existing customers up until the retirement date of November 14, 2018. New customers and current customers will not be able to download SDK anymore via the Azure classic portal. To download you will need to reach out to Azure customer support to receive your generated MFA Service Credentials package. <br> The Microsoft development team is working on changes to MFA by integrating with Azure Multi-Factor Authentication Server SDK.

The Article below will outline the configuration update and steps to enable for a simple switch from Azure MFA SDK to Azure Multi-Factor Authentication Server SDK when released as this will be included in an upcoming hotfix please see [version history](/reference/version-history.md) for announcements. 

## Prerequisites

In order to use Azure Multi-Factor Authentication Server with MIM, you need:

- Internet access from each MIM Service or MFA Server providing PAM and SSPR, to contact the Azure MFA service
- An Azure subscription
- Install is already using Azure MFA SDK
- Azure Active Directory Premium licenses for candidate users, or an alternate means of licensing Azure MFA
- Phone numbers for all candidate users
- MIM hotfix 4.5. or greater see [version history](/reference/version-history.md) for announcements

## Azure Multi-Factor Authentication Server Configuration 
> [!NOTE] 
> In the configuration you will need a valid SSL certificate installed for the SDK. 

### Step 1: Download Azure Multi-Factor Authentication Server from the azure portal 
Sign-in to the [Azure portal](https://portal.azure.com/) and download the Azure MFA server.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### Step 2: Generate activation credentials
Use the **Generate activation credentials to initiate use** link to generate activation credentials. Once Generated save for later use.

### Step 3: Install the Azure Multi-Factor Authentication Server
Once you have downloaded the server, [install](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) it.  Your activation credentials will be required. 

### Step 4: Create your IIS Web Application that will host the SDK
1. Open IIS Manager
![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Create new Website call "MIM MFASDK" , link it to an empty directory 
![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Open Multi-Factor Authentication Console and click on Web Service SDK
![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Once wizards it click through config, Select "MIM MFASDK" and app pool

> [!NOTE] 
> Wizard will require a admin group to be created. More information can be found on the Azure MFA Azure Multi-Factor Authentication Server documentation.
5. Next we need to import the MIM Service account open Multi-Factor Authentication Console select "Users"
    a. Click on "Import from Active Directory"
    b. Navigate to service account aka "contoso\mimservice"
    c. Click "Import" and "Close"
   ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Edit the MIM Service account to Enable in Multi-Factor Authentication Console
![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Update the IIS authentication on the "MIM MFASDK" website. First we will disable the "Anonymous Authentication" then Enable Windows Authentication"
![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Final Step: Add the MIM service account to the "PhoneFactor Admins"
![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## Configuring the MIM Service for Azure Multi-Factor Authentication Server 

### Step 1: Patch Server to 4.5.200.0
 
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


## Next Steps

-    [Getting started with the Azure Multi-Factor Authentication Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [What is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Use Custom Multi-Factor Authentication API to activate PAM or SSPR](Working-with-custommfaserver-for-mim.md)
- [MIM version release history](./reference/version-history.md)