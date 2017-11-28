56271﻿---
# required metadata

title: Identity Manager version history | Microsoft Docs
description: This article documents the various changes made as part of updates to MIM 2016
keywords:
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 11/28/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---

# Version release history

The Microsoft Identity Manager team regularly releases updates. This article is designed to help you keep track of the versions that have been released. You can then decide whether you need to update to the newest version or not.

## 4.4.1749.0

Status: November 30, 2017

[Download](https://www.microsoft.com/download/details.aspx?id=56271)

### Fixed issue

#### Synchronization Service

- Password reset routine fails when Synchronization server domain doesn't have trust relationship with target domain.


#### MIM Service

- Database upgrade failure.A foreign key constraint violation exception is recorded in the database upgrade log
- When you execute self-service password reset requests, the MIM Service randomly stops
- Set calculation fails to reflect correct membership. You cannot delete a binding for an attribute if it's referenced in a dynamic set or group filter,
- The MIM Service did not work for the Request Approval scenario with Exchange Online where approvals are responded to through the MIM Add-in for Outlook
- The msidmPhoneGatePhoneNumber attribute without a Country Code does not use the DefaultCountryCode value in MFASettings.xml
- Set memberships can be updated dynamically without having to rely on the FIM_TemporalEventsJob 
- Synchronization Rules don't support the creation of attribute flow rules for attributes whose names include the hash or pound symbol (#).
  
#### Privilege Access Management 

- New-PAMDomainConfiguration PowerShell cmdlet sets an incorrect value for domain trust configuration resulting in error (This unknown request parameter cannot be processed)

#### Microsoft Identity Portal

- An exception is displayed in the main screen of the Identity Management Portal, and a Close button also appears.
- Buttons displayed incorrectly in Delete Item window.  This issue occurred in Internet Explorer, Firefox, and Chrome. 
- The Lookup button overlaps the Resource Picker button on an Approval activity window in the Authorization workflow. This issue occurred in Internet Explorer, Firefox, and Chrome. 
- In the Group properties popup, the button area overlaps the listview navigation controls on the Delete Members control.  This issue occurred in Internet Explorer, Firefox, and Chrome
- Multiple display elements can’t display correctly.  These include the following:
    - Up and down arrows in some property sheets
    - Empty space at the bottom of some pages and dialog boxes
    - Missing popup overlays
- When you use the filter builder in various areas of the product (such as Advanced Search), the filter builder would get “stuck” if the OK button on a select value dialog box is clicked without an object selected in the add statement area.
- The New Attribute flow popup in a Synchronization Rule edit dialog didn't work as expected in Chrome
- In an object management screen(such as Distribution Groups), if multiple objects are selected by using the check-box and the objects have long display names. Now dialog sizes vertically so that the control doesn’t extend past the end of the browser screen
- In an object management or list screen (such as Distribution Groups), the selected items control may move up the screen to be directly under the last object listed in the table lists
- The filter builder (such as advanced search) in the Safari browser is was nonfunctional
- portal dialog boxes that display attribute values, the shorter words are distributed throughout the cell with lots of white space in between rather than being left-aligned. 
- In some browser versions, the Selected Items is not udpated when the item selection is changed
- Dialog tabs and Copy to Clipboard highlight when navigated to by using the tab key.  
- In Internet Explorer 10, when you view an object grid display (such as Distribution Groups), the “Find the distribution groups you want using the search above” banner overlays part of the button ribbon rather than displaying in the middle of the dialog box.  
- After installing an update to the MIM Portal, the display of the Portal in Internet Explorer fails
- When you use the Advanced Search in the Firefox browser, pressing the enter key on an attribute value field returns an error.  

#### Certificate Management

- A request originator (certificate manager) can't abandon request which is duplicated somehow or just forgotten by user who has Execution permission.
- When you try to renew TPM Virtual Smart Card from the Modern App, a forbidden exception is returned
- During some smart card related activities, existing connections to the CertificateManagement database are left open unexpectedly.  
- If an installation of an update to MIM Certificate Management (CM) is tried before the MIM CM Configuration Wizard being run, the update fails with an exception that appears to be unrelated to the problem.
- The MIM CM Configuration Wizard has incorrect product version information displayed, and the logo isn’t displayed correctly.  
- The exported data for a MIM Certificate Management report differs from the report data.  The column data doesn’t always match the column headings.

## 4.4.1642.0

Status: August 29, 2017

[Download](https://www.microsoft.com/download/details.aspx?id=55794)

### Fixed issue

#### Synchronization Service

- Password reset routine fails when Synchronization server domain doesn't have trust relationship with target domain.
- Declared import filter (checking distinguished name) doesn't work correctly when an object is moved from an organizational unit where it should be filtered to another where it shouldn't be because of using old name of a phantom object.
- Management Agent  Designer hangs on “Configure Partitions and Hierarchies” page
- Precedence doesn't transfer to the next object when the previous one with precedence is disconnected.
- Sun One Management Agent Searching for child containers on the ldap server using paging causes an error if the server doesn't support paging.
- Metaverse object type is changed dynamically causing crash.

#### MIM Service

- Word function does not return empty string if string contains less than number of words
- View members shows incorrect membership when the criteria contains dereferencing and if the dereferencing happens below another criteria piece. 
- AuthZ Workflow denies request with error message 'workflow not found in state persistence store'.
- The workflow runs an enumerate resource activity to query MIM and it is failing intermittently.

#### Privilege Access Management

- Get-PAMRequestToApprove commandlet does not return candidates if the approver not in the role to approve.
- Related MPRs and PAM Navigation Bar node are enabled even when PAM is not installed.
- MIM Service throws exception and logs warning from Domain Configuration synchronizer for PAM.

#### Microsoft Identity Portal

- When using Firefox to access the filter builder on the MIM portal you are unable to use the inner controls (drop-down boxes, text boxes, etc.). They cannot be selected until you first right click (i.e. clicking to display the context menu).
- Portal Search renders incorrectly on some screen resolutions. 
- Calendar Control in Advanced Search is Truncated.
- The filter builder UI was broken – in edit mode (misplaced elements).
- Popup had fixed size and edit controls was not proper sized.
- String cuts for Norway and some other languages in main menu.
- Copied URL from popup not working.

#### Certificate Management

- MIM Self Service Password Portals  

#### Reporting

- Import-FIMReportingSchemaDefinition cmdlet for Reporting fails with error.

#### Client Add-in

- UI Language do not check country code equality

### New features and improvements

#### MIM Service

- Added retry in the longest request processing operations (Validation stage). It is not guarantee that request processing completed but it make request more stable. The fix is disabled by default. To enable the fix you add alwaysOnRetryRequestProcessingTransaction="true" in resourceManagementService section of FIMService config file

#### Certificate Management

- Newer versions of CM Server are able to work with older BulkClient (but not older than 4.4.xxxx.0). 

#### MIM Self Service Password Portals 

- Show warning for QAGate if used provides an answer that contains double-byte character
- Add configurable property to allow enabling/disabling IME usage on SSPR Registration form 
- SSPR masks the Password Registration Questions on Portal Client

## Next steps

- [Support team blog build versions](https://blogs.technet.microsoft.com/iamsupport/idmbuildversions/) includes public release versions of the Identity products, including service packs, updates, rollups and hotfixes of MIM 2016, AAD Connect, FIM 2010 R2, and older build types.
