---
title: ADFS Installation and Configuration for MIM Server
description: ADFS for Microsoft Identity Manager - Step through installation, certificate management, and SharePoint integration using this guide.
services: active-directory
author: Dickson-Mwendia
ms.author: dmwendia
reviewer: markwahl-msft, henrymbuguakiarie
ms.date: 06/30/2025
ms.topic: how-to
ms.service: microsoft-identity-manager
---

# ADFS installation and configuration for MIM Server


Active Directory Federation Services (ADFS) enables secure identity federation and single sign-on for Microsoft Identity Manager (MIM) environments. This guide provides step-by-step instructions for installing and configuring ADFS on a MIM server, including certificate management and SharePoint integration.

## Prerequisites

- Complete the steps in [How to set up ADFS claims-based authentication in MIM](mim-adfs-prepare-authentication.md)


## Set KDS Root Key

We'll use a group managed service account to manage ADFS. To be able to use this account immediately, we must create a KDS root key. Open PowerShell as an administrator and run the following command:

```powershell
Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
```

## Enroll the MIM Server for the ADFS Certificate

1. Right click on the start menu and select **Run**. On the ensuing dialog, type **mmc** and then select **OK**. This launches the **Microsoft Management Console**.
1. Select **File** then **Add/Remove Snap-in**… to launch the Snap-ins window.
1. Select **Certificates** then **Add**. On the window that pops-up, select the **Computer account** radio button then select **Next**.
1. Select **Finish** then **OK**. The Certificates Snap-in should now be available.
1. Expand the Snap-in, then right click on the **Personal** folder. Select **All tasks > Request New Certificate**. A certificate enrollment wizard appears.
1. Select **Next** after reading the getting started guide then click **Next** again to start the enrollment process.
1. On the Request certificates page, you should see the ADFS Certificate template with a "warning" link - _More information is required to enroll for this certificate …_ Select the link.
1. Add the following certificate properties:

   - **a. Subject name**
     - **Type:** Common name - **Value:** adfs.contoso.com
   - **b. Alternative Handling**
     - **Type:** DNS - **Value:** adfs.contoso.com
     - **Type:** DNS - **Value:** certauth.adfs.contoso.com
     - **Type:** DNS - **Value:** enterprisemanagement.contoso.com

1. Select **Apply** then **OK**. Notice that the certificate template warning disappears.
1. Select the ADFS Certificate and click on **Enroll**. Once this process is completed, select **Finish**. This dismisses the configuration wizard.
1. Exit the Snap-in. Feel free to save it if prompted to.

## Install and configure Active Directory Federation Services

1. Launch your Server Manager and select **Add roles and Features** to launch the Add roles and features dialog.
1. Skip the Before you Begin section, if need be, by clicking **Next**.
1. Leave the **Role-based or feature-based installation** radio button selected and click **Next**.
1. Leave the Select a server from the server pool radio button selected. Pick the server you want to install ADFS into (**Example:** mim.contoso.com) and click **Next**.
1. Select Active Directory Federation Services from the list of roles and click **Next**.
1. We won't be adding additional features so click **Next** on the **Features** page.
1. Click **Next** on the information page. Then click **Install**.
1. Once this is complete, click on the _Configure the federation service on this server_ link. The configuration wizard should pop up.
1. Leave the default **Create the first federation server in a federation server farm** radio button selected and click **Next**.
1. Select the account you want to administer ADFS then click **Next**.
1. On the Specify Service Properties page, select the adfs certificate we just created `adfs.contoso.com` and give the federation service a memorable name e.g **Microsoft Identity Manager Portal** and then click **Next**.
1. Create a GMSA account by leaving the default **Create a Group Managed Service Account** radio button checked and provide a name for the account e.g **adfsgmsa** then click **Next**.
1. Leave the default **Create a database on this server using Windows Internal Database** radio button selected and click **Next**.
1. Verify your selections then click **Next**.
1. Verify that all prerequisite checks passed successfully. (Do not worry if you see a warning but be mindful if there are actual errors).
1. Click **Configure**.
1. Verify that you get notified on successful configuration. (Do not worry if there are warnings around SSL and KDS Root Keys).
1. Click **Close**, then **Close** again. Close the server manager and restart your server.

## Install and export the ADFS token signing certificate

1. Launch the ADFS Management tool.
1. In the left-hand navigation pane, expand **Service** then click on **Certificates**. You should see three certificates in the main content area. We are interested in the Token-signing Certificate.
1. Double click the Token-signing certificate. This will launch the Certificate dialog.
1. On the General tab, click **Install Certificate**.
1. Select **Local Machine** then click **Next**.
1. Select **Place all certificates in the following store** then click **Browse**.
1. Select the **Trusted Root Certificate Authorities** store then click **OK** and then **Next**.
1. Click **Finish**. Once the import is successful, click **OK** on the notification dialog.
1. You can verify that this certificate is OK by double clicking on it once more, navigating to **Certification Path** and verifying that the status is **OK**.
1. On the Certificate dialog, click on **Details**.
1. Click **Copy to file…**. An export wizard will pop up. Click **Next**.
1. On the file format page, leave the default **DER encoded binary X.509 (.CER)** and click **Next**.
1. Click on **Browse** and save this certificate somewhere in your file system. Take note of this location since we shall use it later. E.g `C:\\Users\\administrator.contoso\\Documents\\MIM\\ADFSSigningCertificate.cer`.
1. Click **Next** then click **Finish**. Once the export is successful, click **OK** to dismiss the dialog then **OK** to dismiss the certificate window.

## Integrate ADFS with SharePoint Site - MIM Server

### Add a Relying Party Trust

1. Launch the AD FS Management tool.
1. On the left panel, click **Relying Party Trusts**.
1. On the right panel, under **Actions**, click **Add Relying Party Trust…**. This will launch the Add Relying Party Trust Wizard.
1. Leave the **Claims aware** radio button selected and click **Start**.
1. Select the **Enter data about the relying party manually** radio button and click **Next**.
1. In the next section, give your relying party trust a name and a simple description. Take note of this name since we shall use it later.
1. On the token encryption page, click **Next**.
1. Check the **Enable support for the WS-Federation Passive protocol option** and then provide the SharePoint site URL as the **Relying party WS-Federation Passive protocol URL** suffixed with `/_trust/` i.e:

   - `https://mim.contoso.com/_trust/`

1. Provide the string below as the identifier (realm) and click **Add**. Then click **Next**:

   - `urn:contoso:mim`

1. On the access policy page, select **Permit everyone** and then click **Next**.
1. On the next page, confirm your selections and then click **Next**.
1. Leave the option to **Configure the claims issuance for this application** checked and click **Close**. This will trigger a launch of the claim issuance policy dialog where we will add our claims rule.

### Edit Claim Issuance Policy

1. On the claim issuance policy dialog, click **Add Rule**.
1. Leave **Send LDAP Attributes as Claims** selected and click **Next**.
1. Give your rule a name (e.g., MIM Rule) and select **Active directory** as the directory store.
1. On the mapping of LDAP attributes to outgoing claim types, select **User-Principal-Name** as the LDAP Attribute and map it to **UPN** as the Outgoing Claim Type.
1. Click **Finish**, click **Apply** then click **OK**.

### Configure SharePoint Site to Use ADFS

1. Run PowerShell as an administrator and execute the following script. (Go through the commands to ensure you input your own values where appropriate):

   ```powershell
   $signingCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("C:\\Users\\administrator.contoso\\Documents\\MIM\\ADFSSigningCertificate.cer")
   New-SPTrustedRootAuthority -Name "ADFS Certificate" -Certificate $signingCert
   $upn = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -IncomingClaimTypeDisplayName "UPN" -SameAsIncoming
   $realm = "urn:contoso:mim"
   $adfsUrl = "https://adfs.contoso.com/adfs/ls/"
   New-SPTrustedIdentityTokenIssuer -Name "MIM" -Description "Microsoft Identity Manager Portal" -Realm $realm -ImportTrustCertificate $signingCert -ClaimsMappings $upn -SignInUrl $adfsUrl -IdentifierClaim $upn.InputClaimType
   $webApp = Get-SPWebApplication "https://mim.contoso.com"
   $webApp.UseClaimsAuthentication = $true
   $webApp.Update()
   $sptrust = Get-SPTrustedIdentityTokenIssuer -Identity "MIM"
   $trustedAp = New-SPAuthenticationProvider -TrustedIdentityTokenIssuer $sptrust
   $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$true
   Set-SPWebApplication -Identity $webApp -Zone Default -AuthenticationProvider $winAp, $trustedAp
   ```

## Install the MIM Portal

1. Turn off Strong Name Verification before installing this build of the MIM Service and Portal. To do so, launch a terminal as an administrator and run the following commands:

   ```cmd
   reg ADD "HKLM\\Software\\Microsoft\\StrongName\\Verification\\*,*" /f
   reg ADD "HKLM\\Software\\Wow6432Node\\Microsoft\\StrongName\\Verification\\*,*" /f
   ```

1. Then, head over to the MIM Knowledge Base repository and download the latest version of Service and Portal.
1. Download and extract the contents of the en-us folder into a location in your drive.
1. You can then execute the Service and Portal installer using msiexec by running the following command:

   ```cmd
   msiexec /i "Service and Portal.msi" /lvx* log.txt
   ```

Follow these steps after installation to finish setting up your SharePoint site:

1. Add the current signed-in user to the list of users under the default zone in SharePoint Administration, and grant Full Read access.
1. Add the user with a UPN structure to the list of users under the default zone in SharePoint Administration, and grant Full Read access.
1. Set your site to use the authentication method you configured for ADFS. By default, form authentication is used.

### Troubleshooting

In case you encounter the "Cannot reach this page" error, ensure that:

    - You've set the proper bindings on IIS for your site.
    - All your necessary AppPools are up and running.
    - You've attached an SSL certificate.

## Related articles

- [Active Directory Federation Services](/windows-server/identity/ad-fs/ad-fs-overview)
- [Create the Key Distribution Service KDS Root Key](/windows-server/identity/ad-ds/manage/group-managed-service-accounts/group-managed-service-accounts/create-the-key-distribution-services-kds-root-key)
- [Active Directory Certificate Services](/windows-server/identity/ad-cs/active-directory-certificate-services-overview)
- [Implement Federated Authentication in SharePoint Server](/security-for-sharepoint-server/implement-saml-based-authentication-in-sharepoint-server)


