---
# required metadata

title: Identity Manager version history | Microsoft Docs
description: This article documents the various changes made as part of updates to MIM 2016
services: active-directory
documentationcenter: ''
keywords: MIM
author: EugeneSergeev
ms.author: esergeev
reviewer: markwahl-msft
manager: aashiman
ms.date: 2/10/2022
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity

ms.assetid: b0b39631-66df-4c5f-10c9-a1774346f816

ms.reviewer: mwahl
ms.suite: ems

---

# Identity Manager version release history

The Microsoft Identity Manager team regularly releases updates. This article is designed to help you keep track of the versions that have been released. You can then determine whether you are already on the latest service pack and hotfix update version, or if you need to upgrade.

>[!NOTE]
>This article only describes releases of the MIM Sync, Service and Portal, client, CM and PAM components.  Not sure what components you need?  See [Microsoft Identity Manager licensing and downloads](../microsoft-identity-manager-licensing.md).
>
>The version history for Microsoft BHOLD Suite components can be found at [BHOLD modules version release history](version-bhold-history.md).
>
>The version history for the Generic LDAP, Generic SQL, web services, PowerShell, Graph and Lotus Domino connectors can be found at [Connector Version Release History](microsoft-identity-manager-2016-connector-version-history.md).

## MIM Version 4.6.607.0

- Status: February 10, 2022
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=103898)
- [KB article 5009981](https://support.microsoft.com/help/5009981)

This hotfix contains updates for the MIM PAM components, MIM Service and Portal, MIM Synchronization Service, MIM Self-Service Password portals, and also contains cumulative updates to MIM components from the previous hotfixes for MIM 2016 SP2.

The latest MIM CM version is [4.6.359.0](version-history.md#mim-version-463590).

## MIM Version 4.6.540.0

- Status: November 9, 2021
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=103573)
- [KB article 5007373](https://support.microsoft.com/help/5007373)

This hotfix adds defense against XSS (cross-site scripting) for the MIM Portal, and also contains cumulative updates to MIM components from the previous hotfixes for MIM 2016 SP2.

## MIM Version 4.6.531.0

- Status: October 8, 2021
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=103467)
- [KB article 5004861](https://support.microsoft.com/help/5004861)

This hotfix contains updates for the MIM Synchronization Service, MIM Service and MIM PAM components, and also contains cumulative updates to MIM components from the previous hotfixes for MIM 2016 SP2. It also fixes a MIM Service installer issue found in 4.6.530.0 build.

## MIM Version 4.6.530.0

Superseded by 4.6.531.0

## MIM Version 4.6.421.0

- Status: March 17, 2021
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=102786)
- [KB article 4599279](https://support.microsoft.com/help/4599279)
- Full version for Azure AD Premium customers is available for [download from here](https://aka.ms/MIMForAADP)

This hotfix contains updates for the MIM Service and MIM Portal components, and also contains cumulative updates to MIM components from the previous hotfixes for MIM 2016 SP2.

This build introduces Application Context Authentication method to Office 365 mailboxes for the MIM Service component. In order to switch from Basic authentication to Application Context Authentication apply this hotfix first, run a PowerShell script to register an application in Azure AD and reconfigure your MIM Service and Portal using installer's Change mode. Check the [Deployment guide: Installing MIM Service and Portal for Azure AD Premium customers](/microsoft-identity-manager/install-mim-service-portal-azure-ad-premium) for details.

> [!IMPORTANT]
>The hotfix may fail to update MIM Service when Group-Managed Service Account or Office 365 basic authentication method is used and *PollExchangeEnabled* registry key value is set to *1*. As a workaround, set a registry key *HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\FIMService\PollExchangeEnabled* to *0* before applying this hotfix. After the hotfix is installed, revert this value back to *1* and restart MIM Service.

## MIM Version 4.6.359.0

- Status: November 29, 2020
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=102301)
- [KB article 4585922](https://support.microsoft.com/help/4585922)

This hotfix contains updates for the MIM Synchronization Manager, MIM Service and MIM Portal components, and also contains cumulative updates to MIM components from the previous hotfixes for MIM 2016 SP2.
Replaces build 4.6.355.0 to address MIM Service performance issues.

## MIM Version 4.6.355.0

- Status: November 6, 2020

Superseded by 4.6.359.0


## MIM Version 4.6.263.0

- Status: August 7, 2020
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=101612)
- [KB article 4576473](https://support.microsoft.com/help/4576473)

This hotfix contains updates for the MIM CM, MIM Synchronization Manager and PAM components.

## MIM Version 4.6.258.0

- Status: May 22, 2020
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=101305)
- [KB article 4560335](https://support.microsoft.com/help/4560335)

This hotfix contains updates for the MIM Service, MIM Portal and PAM components.

## MIM Version 4.6.34.0

* Status: MIM 2016 Service Pack 2 (SP2) of October, 2019
* Corresponding BHOLD version number: 6.0.62.0
- [Hotfix download](https://www.microsoft.com/download/details.aspx?id=100412)
- [KB article 4512924](https://support.microsoft.com/help/4512924)
- ISO file can be downloaded [from VLSC](https://www.microsoft.com/Licensing/servicecenter/default.aspx) or via  [Visual Studio downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202)

> [!IMPORTANT]
>- .NET Framework 4.6 is required<br>
>- [Visual C++ 2013 Redistributable Package (vcredist_x64.exe or vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784) is required<br>

>[!NOTE]
> Make sure to read [MIM deployment guide](../microsoft-identity-manager-deploy.md) for updated list of pre-requisites and limitations related to TLS 1.2 only environments, Group Managed Service Accounts support and the [upgrade path from previous MIM and FIM versions](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md).

A Service Pack 2 (SP2) rollup package (build 4.6.34.0) is available for Microsoft Identity Manager (MIM) 2016. It is a cumulative update that replaces earlier MIM 2016 SP1 updates 4.4.1302.0 through build 4.5.412.0.
### Updates in MIM 2016 Service Pack 2

#### MIM Client addons
- Added support for MIM Outlook add-on to be loaded into the Outlook for Microsoft 365 Click-To-Run version.

#### Service and Portal
- Added support for MIM Service and Portal to be installed on Windows Server 2019 and use SQL Server 2017, Exchange Server 2019, SharePoint 2019, System Center Service Manager Data Warehouse 2019
- Enabled MIM Service and Portal installation in TLS 1.2 only environments.
- Enabled installation for MIM Service, Password Reset and Password Registration web sites, PAM Monitoring Service, PAM Component Service to use group managed service accounts.
- Added ‘keepSQLjobs’ installer parameter.
- MIM SQL Server Agent temporal jobs no longer start on secondary SQL Always-On Availability Group replicas.
- ‘ExplicitMember.Add’ and ‘ExplicitMember.Remove’ virtual attributes enabled for custom object types on RCDC forms to work with delta changes.

#### Synchronization Service
- Added support for MIM Synchronization Service to be installed on Windows Server 2019, and use SQL Server 2017, Exchange Server 2019
- Enabled MIM Synchronization Service installation in TLS 1.2 only environments.
- Enabled installation for MIM Synchronization Service to use a group managed service account.
- Added ‘Use MIMSync account’ option for MIM Service Management Agent.
 
#### Privilege Access Management 
- PowerShell cmdlet ‘Get-PAMRequest’ returns an additional property 'FIMRequestID'.


## MIM Version 4.5.412.0
> [!IMPORTANT]
>- RCDC form for group objects fails to render when 'displayedOwner' attribute value is not populated. You will not be able to edit/view groups when such an error occurs.<br>

- Availability date: May 10, 2019
- [KB article 4489646](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

This hotfix contains updates for MIM Sync, MIM Service, MIM Portal, MIM CM and PAM components.  It is a cumulative update that replaces earlier MIM 2016 SP1 updates 4.4.1302.0 through build 4.5.286.0.

> [!IMPORTANT]
>- .NET Framework 4.6 is also required for the installer <br>
>- [Visual C++ 2013 x64 Redistributable Packages (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) is required<br>

## MIM Hybrid Reporting Agent Version 3.1.41.0
- Availability date: April 22, 2019
- [Hybrid reporting agent download](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

MIM hybrid reporting agent version 3.1.41.0 includes updates for TLS 1.2 and a bug fix.  The hybrid reporting agent can be used with MIM 2016 SP1 versions 4.4.1302.0 or later and an Azure AD Premium subscription.


## MIM Version 4.5.286.0
- Status: November 19, 2018
- [Hotfix download](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [KB article 4469694](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


This hotfix contains updates for the MIM Service, MIM Portal and PAM components.  .NET Framework 4.6 is required for the installer.


## Version 4.5.202.0
- Status: August 30, 2018
- [KB Release KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- .NET Framework 4.6 is also required for the installer <br>
>- [Visual C++ 2013 x64 Redistributable Packages (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) is required<br>
>- Updated supported locales to new ISO standards ([here](/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- *Denotes New Enhancement 

#### MIM service
- Azure MFA Server integration
    -Alternate Multi-Factor Authentication provider

#### Privilege Access Management 
- PAM REST API could not be started because it could not load file or assembly

#### Microsoft Identity Portal
- Portal are displayed with an incorrect table length
- Advanced Search dialog of the Portal, the scrollbars don’t display properly
- Language Pack Language Pack strong name signature verification failed

#### Certificate Management
- Binding redirect statement for REST API

## Version 4.5.26.0
- Status: June 30, 2018
- [KB Release KB4073679](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- .NET Framework 4.6 is also required for the installer <br>
>- [Visual C++ 2013 x64 Redistributable Packages (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) is required<br>
>- Updated supported locales to new ISO standards ([here](/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- *Denotes New Enhancement 

#### Synchronization service
- *Support for Group Managed Service Accounts
- *Visual Studio Support (Visual Studio 2013,Visual Studio 2015,Visual Studio 2017)
- Updates to MIISACTIVATE.EXE, gMSA Support added 
    - non-gMSA: Miisactivate.exe c:\configBU\miiserver_01.bin “contoso\mimSyncService” *
    - gMSA: Miisactivate.exe c:\configBU\miiserver_01.bin “contoso\mimSyncService”
- Updates to MIISKMU.exe, gMSA Support added 
    - non-gMSA:MIISKMU.exe /e c:\configBU\miiserver_02.bin” /u:”contoso\mimSyncService”
    - gMSA:MIISKMU.exe /e c:\configBU\miiserver_02.bin” /u:”contoso\mimSyncService” *
- Updated partition information is saved as expected when the Refresh then OK buttons are clicked
- When indexing an Indexable String attribute is too long an Unexpected Error was returned, more descriptive error message is now returned
- Creating a Text File management agent when the MIM Synchronization Service is installed on Windows Server 2016, some text encoding options, including Unicode were unavailable
- MIM Service MA If an export error message contains an invalid character, this causes corruption in the run history entries. This build we removed from the error message before being saved to the connector space object and run history

#### MIM service
- *Support for Group Managed Service Accounts
- *Improved Language support to new defined standard
- *FIMAutomation Export-FIMConfig PowerShell cmdlet the “-PamConfig” argument is available to force the PAM configuration objects to be exported
- *FIMAutomation Export-FIMConfig PowerShell cmdlet the “-request” parameter has been added
- *Boolean attributes are always set to NULL upon binding creation , Previous Boolean before hotfix will not be updated
> [!IMPORTANT]
>This can be a breaking change if preforming a configuration migration. Configuration should be evaluated and updated for new feature as configuration migration is considered a new 
    - Implemented initialization of new MIM Boolean attributes to false on creation new object
    - Implemented initialization of new MIM Boolean attributes to false on adding new Boolean attribute binding to the resource
- Customer Experience Improvement Program setting is maintained to false 
- MIM Service installation failed with Database Upgrade error: Cannot insert the value NULL into column 'Name' if not default database name is used
- In hotfix cases the Microsoft 365 setting would be cleared , The encrypted password for the MIM Service’s Exchange Online mailbox is not changed
- *There was no limit to the MIM Service log file created, Updated logging default setting and implemented circular logging capability

#### Privileged Access Management 
- *Support for Group Managed Service Accounts
- *Improved Language support to new defined standard
- Objects that use unmanaged resources are not cleared on time.  these objects will be  properly cleaned up
- *New-PAMRole PowerShell cmdlet the  “-disableAutoApproveIfOwner”  deny self-approval for the role
- * Get-PamRequest PowerShell cmdlet the  “-CreatedFrom” allows for the filtering od PAM specific request
- *PAM Module Additions
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- The warning (Exception: System.ObjectDisposedException: Cannot access a disposed object) will no longer appear in the PAM event log
- Set-PAMUser cmdlet is able to change the PrivAccountName without the delete
- New-PamRole now validates that the “available to” date is greater than the “available from” date
- The “Available From” and “Available To” values are returned by the Get-PAMRole PowerShell cmdlet
- The Get-PamRequest cmdlet filter is now properly
- *Set-PamGroup cmdlet is now able to update the Active Directory shadow principal group object
- Remove-PamUser PowerShell cmdlet fails with an unclear error message, if the user is linked to a Role as a candidate. Now client-side validation was added to the cmdlet, and the exception returned was clarified
- Change Mode PAM accounts are not exposed for configuration
    - PAM Rest API account
    - PAM Component service account
    - PAM Monitoring service account

#### Microsoft Identity Portal
- *Support for Group Managed Service Accounts
- *Improved Language support to new defined standard
- Identity Picker control, the control seems to dynamically grow its width rather than wrapping the text
- Portal, popup dialogs aren’t displayed properly when viewing in Internet Explorer (IE) 10
- Cyrillic symbols in the title bar text is displayed correctly
- Popup windows no longer have the extra scroll bar displaying, when viewed in Internet Explorer
- Failed “Import Workflow Definition” properly throws an exception and recovers, allowing a Synchronization Rule activity to be added to the workflow definition 
- `<httpRuntime enableVersionHeader="false" />` added to default web.config
- Special characters in the distinguishedName no longer prevents Self-Service Password Reset from resetting the user’s password in the Active Directory
- Improvements in the sentences are properly localized in the display
- MIM Add-in for Outlook includes a copy of the missing Outlook interop binaries

#### Certificate Management
- Renewing a virtual smart card through the MIM CM Modern App, user receives Forbidden exception
- *Improved Language support to new defined standard
- PIN Utility “CLM has encountered an error while trying to change Smart Card PIN.  Wrong number of Arguments or Invalid Property Assignment.”
- Update to the MIM Certificate Authority Modules from 4.4.1302.0 to a build later than 4.4.1459, the setup fails
- Modern App for Renew, Enroll, and Replace operations, the request history doesn’t contain all request status items as are recorded
- Online Update doesn’t complete and returns the exception “Record has been updated or deleted by another user.”  
- The “Download Certificate” link in the Certificate Management Portal, the certificate download (.cer file) was too large
- MIM Certificate Management Bulk Client will work with both TLS 1.1 and TLS 1.2.  

 
## Version 4.4.1749.0

- Status: November 30, 2017
- [Download](https://www.microsoft.com/download/details.aspx?id=56271)

### Fixed issues
The following issues have been fixed in MIM version 4.4.1749.0.

#### Synchronization service

- Password reset routine fails when Synchronization server domain doesn't have trust relationship with target domain.

#### MIM service

- Database upgrade failure.A foreign key constraint violation exception is recorded in the database upgrade log.
- When you execute self-service password reset requests, the MIM Service randomly stops.
- Set calculation fails to reflect correct membership. You cannot delete a binding for an attribute if it's referenced in a dynamic set or group filter.
- The MIM Service did not work for the Request Approval scenario with Exchange Online where approvals are responded to through the MIM Add-in for Outlook.
- The msidmPhoneGatePhoneNumber attribute without a Country Code does not use the DefaultCountryCode value in MFASettings.xml.
- Set memberships can be updated dynamically without having to rely on the FIM_TemporalEventsJob.
- Synchronization Rules don't support the creation of attribute flow rules for attributes whose names include the hash or pound symbol (#).
  
#### Privilege Access Management 

- New-PAMDomainConfiguration PowerShell cmdlet sets an incorrect value for domain trust configuration resulting in error (This unknown request parameter cannot be processed).

#### Microsoft Identity Portal

- An exception is displayed in the main screen of the Identity Management Portal, and a Close button also appears.
- Buttons displayed incorrectly in Delete Item window.  This issue occurred in Internet Explorer, Firefox, and Chrome. 
- The Lookup button overlaps the Resource Picker button on an Approval activity window in the Authorization workflow. This issue occurred in Internet Explorer, Firefox, and Chrome. 
- In the Group properties popup, the button area overlaps the listview navigation controls on the Delete Members control.  This issue occurred in Internet Explorer, Firefox, and Chrome.
- Multiple UI elements aren't displayed correctly. The following elements are fixed:

    - Up and down arrows in some property sheets.
    - Empty space at the bottom of some pages and dialog boxes.
    - Missing popup overlays.

- When you use the filter builder in various areas of the product (such as Advanced Search), the filter builder would get “stuck” if the OK button on a select value dialog box is clicked without an object selected in the add statement area.
- The New Attribute flow popup in a Synchronization Rule edit dialog didn't work as expected in Chrome.
- In an object management screen(such as Distribution Groups), if multiple objects are selected by using the check-box and the objects have long display names. Now dialog sizes vertically so that the control doesn’t extend past the end of the browser screen.
- In an object management or list screen (such as Distribution Groups), the selected items control may move up the screen to be directly under the last object listed in the table lists.
- The filter builder (such as advanced search) in the Safari browser is nonfunctional.
- portal dialog boxes that display attribute values, the shorter words are distributed throughout the cell with lots of white space in between rather than being left-aligned. 
- In some browser versions, the Selected Items is not updated when the item selection is changed.
- Dialog tabs and Copy to Clipboard highlight when navigated to by using the tab key.  
- In Internet Explorer 10, when you view an object grid display (such as Distribution Groups), the “Find the distribution groups you want using the search above” banner overlays part of the button ribbon rather than displaying in the middle of the dialog box.  
- After installing an update to the MIM Portal, the display of the Portal in Internet Explorer fails.
- When you use the Advanced Search in the Firefox browser, pressing the enter key on an attribute value field returns an error.  

#### Certificate Management

- A request originator (certificate manager) can't abandon a request that's duplicated or forgotten by a user who has Execution permission.
- When you try to renew TPM Virtual Smart Card from the Modern App, a forbidden exception is returned.
- During some smart card activities, existing connections to the CertificateManagement database are left open unexpectedly.  
- If an installation of an update to MIM Certificate Management (CM) is tried before the MIM CM Configuration Wizard being run, the update fails with an exception that appears to be unrelated to the problem.
- The MIM CM Configuration Wizard has incorrect product version information displayed, and the logo isn’t displayed correctly.  
- The exported data for a MIM Certificate Management report differs from the report data.  The column data doesn’t always match the column headings.

## Version 4.4.1642.0

- Status: August 29, 2017
- [Download](https://www.microsoft.com/download/details.aspx?id=55794)

### Fixed issues
The following issues have been fixed in MIM version 4.4.1642.0.

#### Synchronization Service

- Password reset routine fails when Synchronization server domain doesn't have trust relationship with target domain.
- Declared import filter (checking distinguished name) doesn't work correctly when an object is moved from an organizational unit where it should be filtered to another where it shouldn't be because of using old name of a phantom object.
- Management Agent  Designer hangs on “Configure Partitions and Hierarchies” page.
- Precedence doesn't transfer to the next object when the previous one with precedence is disconnected.
- Sun One Management Agent Searching for child containers on the LDAP server using paging causes an error if the server doesn't support paging.
- Metaverse object type is changed dynamically causing crash.

#### MIM Service

- Word function does not return empty string if string contains less than number of words.
- View members UI shows incorrect membership when the criteria contains dereferencing and if the dereferencing happens below another criteria piece. 
- AuthZ Workflow denies request with error message 'workflow not found in state persistence store'.
- The workflow runs an enumerate resource activity to query MIM and it is failing intermittently.

#### Privilege Access Management

- Get-PAMRequestToApprove commandlet does not return candidates if the approver not in the role to approve.
- Related MPRs and PAM Navigation Bar node are enabled even when PAM is not installed.
- MIM Service throws exception and logs warning from Domain Configuration synchronizer for PAM.

#### Microsoft Identity Portal

- When using Firefox to access the filter builder on the MIM portal you are unable to use the inner controls (drop-down boxes, text boxes, and so on). They cannot be selected until you first right click (that is,  clicking to display the context menu).
- Portal Search renders incorrectly on some screen resolutions. 
- Calendar Control in Advanced Search is Truncated.
- The filter builder UI was broken – in edit mode (misplaced elements).
- Popup had fixed size and edit controls was not proper sized.
- String cuts for Norway and some other languages in main menu.
- Copied URL from popup not working.

#### Certificate Management

- MIM Self Service Password Portals.

#### Reporting

- Import-FIMReportingSchemaDefinition cmdlet for Reporting fails with error.

#### Client add-in

- UI Language does not check country code equality.

### New features and improvements
The following features and improvements have been added in MIM version 4.4.1642.0.

#### MIM Service

- Added retry in the longest request processing operations (Validation stage). It doesn't guarantee that request processing is completed, but makes requests more stable. The fix is disabled by default. To enable the fix you add alwaysOnRetryRequestProcessingTransaction="true" in resourceManagementService section of FIMService config file

#### Certificate Management

- Newer versions of CM Server are able to work with older BulkClient (but not older than 4.4.xxxx.0). 

#### MIM Self-service Password Portals 

- Show warning for QAGate if used provides an answer that contains double-byte character
- Add configurable property to allow enabling/disabling IME usage on SSPR Registration form 
- SSPR masks the Password Registration Questions on Portal Client

## Version 4.4.1549.0
 
* Status: previous MIM 2016 SP1 Hotfix Update, of March 27, 2017
* Corresponding BHOLD version number: 6.0.36.0
* Read more at [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## Version 4.4.1302.0

* Status: MIM 2016 Service Pack 1 (SP1) of November 9, 2016
* Corresponding BHOLD version number: 5.0.3355.0

### Updates in this service pack


#### MIM

- **MIM Portal cross-browser compatibility for end-user self-service:** In this Service Pack we are introducing support for most major browsers. Users may now access and interact with the MIM Portal for self-service group and profile management from Edge, Chrome, and Safari.

- **MIM Service support for Exchange Online:** The MIM Service has long supported sending and receiving emails for approvals and notifications. Prior to SP1 MIM only supported Exchange Server or SMTP. With service pack 1, the MIM Service can send and receive requests as well as email notifications using a Microsoft 365 Exchange online account.

- **Image file format validation on upload:** MIM is now able to validate the file format of images when they are uploaded to the portal.

#### Privileged Access Management(PAM)

- **PAM "PRIV" (bastion) forest support for Windows Server 2016 functional level:** The MIM PAM Service may be configured in an environment with domain controllers running at the Active Directory Domain Services forest functional level of Windows Server 2016. When configured, a user’s Kerberos ticket will be time-limited to the remaining time of their role activation.

    >[!Note]
    >If you choose to maintain the forest functional level of Windows Server 2012 R2 in your CORP domain, it is recommended to install [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) and [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) on the CORP domain controller.

- **Privileged account elevation into groups exclusive to the “PRIV” (bastion) forest:** Now, administrators can inform the MIM Service of groups and users exclusive to the “PRIV” forest. Doing this allows these groups and users to be included in PAM roles.  They can then be activated for a role and assigned membership to groups in the “PRIV” forest.

- **PAM Deployment Scripts:** PAM Deployment Scripts allow administrators to streamline the installation of the PAM environment.

- **PAM Cmdlets for Authentication Policy Silo configuration:** Service pack 1 introduces new Cmdlets to harden the security of your bastion forest. These Cmdlets automatically create an Authentication Policy Silo, bound to an Authentication Policy Template.

    >[!Note]
    > These Cmdlets run automatically as part of the deployments scripts.



### Platform support 

Updated platform support information may be found in the document called [Supported platforms for MIM 2016](../microsoft-identity-manager-2016-supported-platforms.md).  New platforms supported in this service pack include SQL Server 2016, SharePoint 2016.


### Issues fixed in this release


#### PAM
- New-PAMGroup did not create MIM objects for domain local groups in the PRIV forest
- New-PAMDomainConfiguration would fail with a “netdom” error message
- PAM Monitoring Service logged warnings for groups in the PRIV forest


### How to upgrade

Customers upgrading to Microsoft Identity Manager 2016 Service Pack 1 should follow the below guidance on all services applicable to their deployment.

>[!Note]
>Customers running Forefront Identity Manager 2010 R2 SP1 or earlier must first upgrade their environment to Microsoft Identity Manager 2016 released in August of 2015, then follow the steps below.

Before you begin

You need to upgrade the MIM Sync engine prior to upgrading the MIM service and portal.
You need to backup the MIMService and MIM Sync databases.

1. Uninstall the Microsoft Identity Manager component you are upgrading
2. Once the uninstall completes, open the splash page located on your installation media “FIMSplash.htm”
3. Select the MIM component to upgrade
4. Proceed with the installation following the prompts
   * MIM Service & Portal Installation: When choosing the Exchange Online as the mail account, enter the email address and credentials of the Exchange Online account on the next screen.

## Version 4.3.2266.0


* Status: MIM 2016 Hotfix of July 15, 2016; customers must upgrade to a more recent version to stay supported
* Read more at [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## Version 4.3.2195.0

* Status: MIM 2016 Hotfix of March 20, 2016, customers must upgrade to a more recent version to stay supported
* Corresponding BHOLD version number: 5.0.3355.0
* Read more at [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
	
## Version 4.3.2064.0

* Status MIM 2016 Hotfix of December 11, 2015; customers must upgrade to a more recent version to stay supported
* Corresponding BHOLD version number: 5.0.3176.0
* Read more at [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## Version 4.3.1935.0

* Status MIM 2016 GA of August 6, 2015; customers must upgrade to a more recent version to stay supported
* Corresponding BHOLD version number: 5.0.3079.0

