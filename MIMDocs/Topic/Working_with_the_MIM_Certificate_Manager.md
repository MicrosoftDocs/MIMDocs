---
title: Working with the MIM Certificate Manager
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
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
author: Kgremban
---
# Working with the MIM Certificate Manager
After you have MIM 2016 and Certificate Manager up and running, you can deploy the MIM Certificate Manager Windows store application so that your users can easily manage their physical smart cards, virtual smart cards and software certificates. The steps to deploy MIM CM  app are as follows:

1.  Create a certificate template.

2.  Create a profile template.

3.  Prepare the app.

4.  Deploy the app via SCCM or Intune.

## Create a certificate template
You create a certificate template for the CM  app the same way you ordinarily would, except that you have to make sure that the certificate template is version 3 and up.

1.  Log into the server running AD CS (the certificate server).

2.  Open the MMC.

3.  Click **File &gt; Add/Remove Snap-in**.;

4.  In the Available snap-ins list, click **Certificate Templates** and then click **Add**.

5.  You will now see **Certificate Templates** under **Console Root** in the MMC. Double click it to view all the available certificate templates.

6.  Right-click the **Smartcard Logon** template and click **Duplicate Template**.

7.  On the Compatibility tab, under Certification Authority select Windows Server 2008 and under Certificate Recipient select Windows 8.1/Windows Server 2012 R2. 
    This step is crucial because it makes sure that you have a version 3 (or higher) certificate template, and only version 3 works with the certificate manager app. Because the version is set the first time you create and save the certificate template, if you didn’t create the certificate template in this way there is no way to modify it to the correct version and you should create a new one before proceeding.

8.  On the **General** tab, in the **Display Name** field, type the name you want to appear in the app's UI, such as **Virtual Smart Card Logon**.

9. On the **Request Handling** tab, set the **Purpose** to **Signature and encryption** and under **Do the following…** select **Prompt the user during enrollment**.

10. On the **Cryptography** tab under **Provider Category**, select **Key Storage Provider and Requests can use any provider available on the subject’s computer**.

    > [!NOTE]
    > You will only see Key Storage Provider as an option if your template is version 3. If you don’t see it, you probably didn’t create the certificate template correctly with the correct version. Start over with step 5, above.

11. On the **Security** tab, add the security group that you want to provide **Enroll** access for. For example, if you want to provide access to all users, select the **Authenticated users** group and then select **Enroll permissions** for them.

12. Click **OK** to finalize your changes and create the new template. You should be able to see your new template in the list of Certificate Templates.

13. Select **File** and click **Add/Remove Snap-in** to add the Certification Authority snap-in to your MMC console. When asked which computer you want to manage, select **Local Computer**.

14. In the left pane of the MMC, expand **Certification Authority (Local)** and then expand your CA within the Certification Authority list.

15. Right-click **Certificate Templates**, click **New &gt; Certificate Template** to Issue.

16. From the list select the new template you created and click **OK**.

## Create a profile template
Make sure when you create a profile template to set it to create/destroy the vSC and to remove the data collection. The CM app cannot handle collected data, so it’s important to disable it, as follows.

1.  Log into the CM portal as a user with administrative privileges.

2.  Go to Administration &gt; Manage Profile templates and make sure that the box is checked next to MIM CM Sample Smart Card Logon Profile Template and then click on Copy a selected profile template.

3.  Type the name of the profile template and click **OK**.

4.  In the next screen, click **Add new certificate template** and make sure to check the box next to the CA name.

5.  Check the box next to the name of the profile template **Logon** and click **Add**.

6.  Remove the SmartCardLogon template by checking the box next to it and then clicking **Delete selected certificate templates** and then **OK**.

7.  Scroll down to the bottom and click **Change settings**.

8.  Check the boxes next to **Create/Destroy virtual smart card** and **Diversify Admin Key**.

9. Under **User PIN Policy** select **User Provided**.

10. In the left pane, click **Renew Policy &gt; Change general settings**. Select **Reuse card on renew** and click **OK**.

11. You have to disable data collection items for each and every policy by clicking on the policy in the left pane, and then checking the box next to **Sample data item** and then click **Delete data collection items**. Then click **OK**.

## Prepare the CM app for deployment

1.  In the command prompt, run the following command to unpack the app and extract the content into a new subfolder named appx and create a copy so that you don’t modify the original file.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  In the appx folder, change the name of the file called CustomDataExample.xml to Custom.data

3.  Open the Custom,data file and modify the parameters as necessary.

    |||
    |-|-|
    |MIMCM URL|The FQDN of the portal you used to configure CM. For example, https://mimcmServerAddress/certificatemanagement|
    |ADFS URL|If you will be using AD FS, insert your AD FS URL. For example, https://adfsServerSame/adfs|
    |PrivacyUrl|You can include an URL to a web page explaining what you do with the user details collected for certificate enrollment.|
    |SupportMail|You can include an email address for support issues.|
    |LobComplianceEnable|You can set this to true or false. By default it's set to true.|
    |MinimumPinLength|By default it's set to 6.|
    |NonAdmin|You can set this to true or false. By default it's set to false. Only modify this if you want users who are not admins on their computers to be able enroll and renew certificates.|

4.  Save the file and exit the editor.

5.  Signing the package creates a signing file, so you have to delete the original signing file named AppxSignature.p7x.

6.  The AppxManifest.xml file specifies the subject name of the signing certificate. Open this file to edit it.

7.  You need to obtain a signing certificate before starting this section. See below, Enabling smartcard renewal for non-admins in MIM 2016 Certificate Manager, step 1.

8.  In the &lt;Identity&gt; element, modify the value of the Publisher attribute to be identical to the subject listed in your signing certificate, for example “CN=SUBJECT”.

9. Save the file and exit the editor.

10. In the command prompt, run the following command to repack and sign the .appx file.

    ```
    cd .. 
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    where app package name is the same name you used when you created the copy.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    This provides the new .appx file. The pfx file provides a location for the signing certificate and a password for the .pfx file.

11. To work with AD FS Authentication:

    -   Open the Virtual Smart Card application. This makes it easier for you to find the values needed for the next step.

    -   To add the application as a client onto the AD FS server and configure CM on the server, on the AD FS server, open Windows PowerShell and run the command `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

        The following is the ConfigureMimCMClientAndRelyingParty.ps1 script:

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri 
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   Update the values of redirectUri and serverFQDN.

    -   To find the redirectUri, in the Virtual Smart Card application, open the application settings panel, click **Settings**, and the redirect URI should be listed under the AD FS server address bar. The URI will only appear if an ADFS server address is configured.

    -   The serverFQDN, is the MIMCM server full computer name only.

    -   For help with the **ConfigureMIimCMClientAndRelyingParty.ps1** script, run `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`

## Deploy the app
When you set up the CM app, in the Download Center, download the file MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip and extract all its contents. The .appx file is the installer. You can deploy it in any way you ordinarily deploy Windows store apps, using [System Center Configuration Manager](https://technet.microsoft.com/en-us/library/dn613840.aspx), or [Intune](https://technet.microsoft.com/en-us/library/dn613839.aspx) to sideload the app so that users will have to access it through the Company Portal or will get it pushed directly to their machines.

