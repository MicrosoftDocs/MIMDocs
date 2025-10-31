---
# required metadata

title: Identity Manager BHOLD version history | Microsoft Docs
description: This article documents the various changes made as part of updates to BHOLD within MIM 2016
keywords:
author: henrymbuguakiarie
ms.author: henrymbugua
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid:

---

# BHOLD modules version release history

The Microsoft Identity Manager team regularly releases updates that are listed in the [version](version-history.md) history. This article helps you keep track of the versions that have been released for BHOLD, a subcomponent of the Microsoft Identity Manager. You can then decide whether you need to update to the newest version or not.  Microsoft does not recommend customers start new deployments of the Microsoft BHOLD Suite components.

## Silverlight end of support

The BHOLD Model Generator, BHOLD Analytics and BHOLD FIM Integration modules will be removed from BHOLD, as these modules have a dependency on Microsoft Silverlight, which will reach the [end of support](https://support.microsoft.com/windows/silverlight-end-of-support-0a3be3c7-bead-e203-2dfd-74f0a64f1788) on October 12, 2021.


## Version 6.0.62.0

- Status: October 2018
- [Download](https://www.microsoft.com/download/details.aspx?id=55950)

## Version 6.0.36.0

- Status: September 7, 2017

### Enhancements 
BHOLD version 6.0.36.0 includes the following enhancements:

- Platform update x86 to x64.

### Fixed issues
The following issues have been fixed in BHOLD version 6.0.36.0.

#### BHOLD Core

- Installer updated for x86 to x64.
- Web interface does not show all the permissions.
- Fixes to the B1Common library.
- User can't assign Role to OrgUnit when Role has few permissions with cardinality.
- Permission cardinality does not work for Max. users numbers.

#### Access Management Connector

- n/a

#### BHOLD integration module

- Installer updated for x86 to x64.
- LAYOUT folder incorrect location.

#### BHOLD model generator

- Model roles from parent departments are not inherited.

#### BHOLD analytics

- n/a

#### BHOLD attestation

- n/a

#### BHOLD reporting

- Custom attribute 'employeeID' not available.
- Unable to properly load large data using 3-file set.

## Next steps

- [BHOLD concepts guide](../bhold/bhold-concepts-guide.md)
- [BHOLD installation guide](../bhold/bhold-installation-guide.md)
- [BHOLD developer reference](mim2016-bhold-developer-reference.md)
- [MIM version history](version-history.md)

