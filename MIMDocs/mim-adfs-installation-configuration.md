---
title: How to install and configure ADFS for MIM server
description: ADFS for Microsoft Identity Manager - Step through installation, certificate management, and SharePoint integration using this guide.
services: active-directory
author: Dickson-Mwendia
ms.author: dmwendia
reviewer: markwahl-msft, henrymbuguakiarie
ms.date: 06/30/2025
ms.topic: how-to
ms.service: microsoft-identity-manager
---

# How to install and configure ADFS for MIM server

Active Directory Federation Services (ADFS) lets you set up secure identity federation and single sign-on for Microsoft Identity Manager (MIM) environments. This article gives step-by-step instructions to install and set up ADFS on a MIM server, including certificate management and SharePoint integration.

## Prerequisites

- Complete the steps in [How to set up ADFS claims-based authentication in MIM](mim-adfs-prepare-installation.md)

## Set the Key Distribution Service (KDS) root key

This article uses a group managed service account to manage ADFS. To use this account right away, create a KDS root key.

1. Open PowerShell as an admin.
1. Run the following command:

```powershell
Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
```

## Enroll the MIM server for the ADFS certificate

1. Open the start menu, then select **Run**. In the dialog, enter *mmc* and select **OK**. The **Microsoft Management Console** opens.
1. Select **File**, then select **Add/Remove Snap-in** to open the Snap-ins window.
1. Select **Certificates**, then select **Add**. In the window that appears, select the **Computer account** radio button, then select **Next**.
1. Select **Finish**, then select **OK**. The Certificates Snap-in is now available.
1. Expand the Snap-in, then open the **Personal** folder. Select **All tasks > Request New Certificate**. The certificate enrollment wizard appears.
1. After you read the getting started guide, select **Next**, then select **Next** again to start the enrollment process.
1. On the Request certificates page, you see the ADFS Certificate template with a "warning" link *More information is required to enroll for this certificate*. Select the link.
1. Add the following certificate properties:

   - **a. Subject name**
     - **Type:** Common name - **Value:** adfs.contoso.com
   - **b. Alternative handling**
     - **Type:** DNS - **Value:** adfs.contoso.com
     - **Type:** DNS - **Value:** certauth.adfs.contoso.com
     - **Type:** DNS - **Value:** enterprisemanagement.contoso.com

1. Select **Apply**, then select **OK**. The certificate template warning disappears.
1. Choose the ADFS Certificate, then select **Enroll**. When the process is complete, select **Finish** to dismiss the configuration wizard.
1. Exit the Snap-in. You can save it if you're prompted.

## Install and configure Active Directory Federation Services

1. Open Server Manager and select **Add roles and Features** to launch the Add roles and features dialog.
1. If needed, skip the Before you Begin section by selecting **Next**.
1. Leave the **Role-based or feature-based installation** radio button selected and select **Next**.
1. Leave the Select a server from the server pool radio button checked. Select the server to install ADFS on (for example, mim.contoso.com), and select **Next**.
1. Select Active Directory Federation Services from the list of roles and select **Next**.
1. Don't add additional features. Select **Next** on the **Features** page.
1. On the information page, select **Next**, and then select **Install**.
1. When the installation finishes, select the  *Configure the federation service on this server* link. The configuration wizard opens.
1. Leave the default **Create the first federation server in a federation server farm** radio button checked and select **Next**.
1. Select the account to administer ADFS, and then select **Next**.
1. On the Specify Service Properties page, select the ADFS certificate you created (`adfs.contoso.com`), enter a memorable federation service name (for example, Microsoft Identity Manager Portal), and then select **Next**.
1. Leave the default **Create a Group Managed Service Account** radio button selected, enter a name for the account (for example, adfsgmsa), and then select **Next**.
1. Leave the default **Create a database on this server using Windows Internal Database** radio button selected and select **Next**.
1. Verify your selections, and then select **Next**.
1. Check that all prerequisite checks pass. You might see some warnings, but focus on any errors.
1. Select **Configure**.
1. Check for a notification that configuration is successful. Don't worry if you see warnings about SSL or KDS Root Keys.
1. Select **Close**, and then **Close** again. Close the Server Manager and restart your server.

## Install and export the ADFS token signing certificate

1. Launch the ADFS Management tool.
1. In the navigation pane, expand **Service**, then select **Certificates**. You see three certificates in the main content area. Focus on the token-signing certificate.
1. Double-click the token-signing certificate. The certificate dialog opens.
1. On the General tab, select **Install Certificate**.
1. Select **Local Machine**, then select **Next**.
1. Select **Place all certificates in the following store**, then select **Browse**.
1. Select the **Trusted Root Certificate Authorities** store, then select **OK**, and then select **Next**.
1. Select **Finish**. When the import is successful, select **OK** on the notification dialog.
1. To verify that the certificate is OK, select it again, go to **Certification Path**, and verify that the status is **OK**.
1. On the Certificate dialog, select **Details**.
1. Select **Copy to file…**, then select **Next** in the export wizard that appears.
1. On the file format page, keep the default **DER encoded binary X.509 (.CER)** option, then select **Next**.
1. Select **Browse** and save this certificate somewhere in your file system. Take note of this location since we use it later. For example,`C:\\Users\\administrator.contoso\\Documents\\MIM\\ADFSSigningCertificate.cer`.
1. Select **Next**, then select **Finish**. When the export is successful, select **OK** to close the dialog, then select **OK** to close the certificate window.

## Integrate ADFS with SharePoint site - MIM server

### Add a Relying Party Trust

1. Open the AD FS Management tool.
1. In the left panel, select **Relying Party Trusts**.
1. In the right panel, under **Actions**, select **Add Relying Party Trust…**. The Add Relying Party Trust Wizard opens.
1. Leave the **Claims aware** radio button checked and select **Start**.
1. Select the **Enter data about the relying party manually** radio button, and select **Next**.
1. In the next section, enter a name and a simple description for the relying party trust. Note this name because you use it later.
1. On the token encryption page, select **Next**.
1. Select the **Enable support for the WS-Federation Passive protocol option**, and enter the SharePoint site URL as the **Relying party WS-Federation Passive protocol URL** with `/_trust/` at the end. The URL is in the form `https://mim.contoso.com/_trust/`.
1. Enter `urn:contoso:mim` as the identifier (realm), select **Add**, and then select **Next**.
1. On the access policy page, select **Permit everyone**, and then select **Next**.
1. On the next page, confirm your selections, and then select **Next**.
1. Leave the option to **Configure the claims issuance for this application** checked, and select **Close**. The claim issuance policy dialog opens, where you add the claims rule.

### Edit claim issuance policy

1. In the claim issuance policy dialog, select **Add Rule**.
1. Leave **Send LDAP Attributes as Claims** checked, and select **Next**.
1. Enter a name for the rule (for example, MIM Rule), and select **Active directory** as the directory store.
1. In the mapping of LDAP attributes to outgoing claim types, select **User-Principal-Name** as the LDAP attribute, and map it to **UPN** as the outgoing claim type.
1. Select **Finish**, then **Apply**, then **OK**.

### Configure SharePoint site to use ADFS

1. Run PowerShell as an admin, and run the following script. Replace the placeholder values with your own where needed.

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

1. Open the MIM Knowledge Base repository and download the latest version of Service and Portal.
1. Download and extract the contents of the en-us folder into a location in your drive.
1. Run the service and portal installer using msiexec by running the following command:

   ```cmd
   msiexec /i "Service and Portal.msi" /lvx* log.txt
   ```

After installation, follow these steps to finish setting up your SharePoint site:

1. Add the current signed-in user to the list of users under the default zone in SharePoint Administration, and grant Full Read access.
1. Add the user with a UPN structure to the list of users under the default zone in SharePoint Administration, and grant Full Read access.
1. Set your site to use the authentication method you configured for ADFS. By default, form authentication is used.

### Troubleshooting

In case you encounter the "Cannot reach this page" error, ensure that:

- You set the proper bindings on IIS for your site.
- All your necessary AppPools are up and running.
- You attach an SSL certificate.

## Related articles

- [Active Directory Federation Services](/windows-server/identity/ad-fs/ad-fs-overview)
- [Create the Key Distribution Service KDS Root Key](/windows-server/identity/ad-ds/manage/group-managed-service-accounts/group-managed-service-accounts/create-the-key-distribution-services-kds-root-key)
- [Active Directory Certificate Services](/windows-server/identity/ad-cs/active-directory-certificate-services-overview)
- [Implement Federated Authentication in SharePoint Server](/sharepoint/security-for-sharepoint-server/implement-saml-based-authentication-in-sharepoint-server)