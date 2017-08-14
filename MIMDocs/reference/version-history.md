---
# required metadata

title: Identity Manager version history | Microsoft Docs
description: This article documents the various changes made as part of updates to MIM 2016
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/14/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---

# Version release history

The Microsoft Identity Manager team reguluarly releases updates. This article is designed to help you keep track of the versions that have been released, and to understand whether you need to update to the newest version or not.

## 4.4.1609.0

Status: July 23 2017

### Fixed issue

#### Syncronization Service

- Password reset routine fails when Synchronization server domain doesn't have trust relationship with target domain.
- Declared import filter (checking distinguished name) doesn't work correctly when object has been moved from organization unit where it should be filtered to another one where it shouldn't because of using old name of a phantom object.
- Management Agent  Designer hangs on “Configure Partitions and Hierarchies” page
- Precedence doesn't transfer to the next object when the previous one with precedence is disconnected.
- Sun One Management Agent Searching of a child containers on ldap server using paging causes error if server doesn't support paging.
- Metaverse object type is changed dynamically causing crash.

#### MIM Service

- Word function does not return empty string if string contains less than number of words
- View members shows incorrect membership when the criteria contains dereferencing and if the dereferencing happens below another criteria piece. 
- AuthZ Workflow denies request with error message 'workflow not found in state persistence store'.
- The workflow runs an enumerate resource activity to query MIM and it is failing intermittently.

#### Privledge Access Management

- Get-PAMRequestToApprove commandlet does not return candidates if the approver not in the role to approve.
- Related MPRs and PAM Navigation Bar node are enabled even when PAM is not installed.
- MIM Service throws exception and logs warning from Domain Configuration synchronizer for PAM.

#### Microsoft Identity Portal

- When using the filter-builder in the MIM Portal from Firefox, the inner controls (drop-down boxes, text boxes, etc.) cannot be selected without first right-mouse button click (i.e. clicking to display the context menu) first.
- Portal Search renders incorrectly on some screen resolutions. 
- Calendar Control in Advanced Search is Truncated.
- filterbuilder UI was broken – in edit mode (misplaced elements).
- Popup had fixed size and edit controls was not proper sized.
- Stringcuts for Norway and some other languages in main menu.
- Copied URL from popup not working.

#### Certificate Management

- MIM Self Service Password Portals  

#### Reporting

- Import-FIMReportingSchemaDefinition cmdlet for Reporting fails with error.

#### Client Add-in

- UI Language do not check country code equality

### New features and improvements

#### Syncronization Service

#### MIM Service

- Added retry in the longest request processing operations (Validation stage). It is not guarantee that request processing completed but it make request more stable. The fix is disabled by default.To enable the fix it is necessary to add alwaysOnRetryRequestProcessingTransaction="true" in resourceManagementService section of FIMService config file

#### Certificate Management

- Newer versions of CM Server will be able to work with older BulkClient (but not older then 4.4.xxxx.0). 

#### MIM Self Service Password Portals 

- Show warning for QAGate if used provides an answer that contains double-byte character
- Add configurable property to allow enabling/disabling IME usage on SSPR Registration form 
- SSPR masks the Password Registration Questions on Portal Client

## Next steps

- [Support team blog build versions](https://blogs.technet.microsoft.com/iamsupport/idmbuildversions/) includes public release versions of the Identity products, including service packs, updates, rollups and hotfixes of MIM 2016, AAD Connect, FIM 2010 R2, and older build types.