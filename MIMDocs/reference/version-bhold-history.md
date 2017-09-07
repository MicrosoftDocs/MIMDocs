﻿---
# required metadata

title: Identity Manager BHOLD version history | Microsoft Docs
description: This article documents the various changes made as part of updates to BHOLD within MIM 2016
keywords:
author: fimguy
ms.author: fimguy
manager: binbin
ms.date: 09/07/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---

# BHOLD Modules Version release history

The Microsoft Identity Manager team regularly releases updates listed [here](version-history.md). This article helps you keep track of the versions that have been released for BHOLD. A subcomponent of the Microsoft Identity Manager. You can then decide whether you need to update to the newest version or not.

## 6.0.36.0

Status: Sept 7, 2017

[Download (Pending Release)]()

### Fixed issue

#### BHOLD Core

- Installer updated for x86 to x64
- Web interface does not show all the permissions
- Fixes to the B1Common library
- User can't assign Role to OrgUnit when Role has few permissions with cardinality
-Permission cardinality does not work for Max. users numbers

#### Access Management Connector

#### BHOLD Integration Module

- Installer updated for x86 to x64
- LAYOUT folder incorrect location

#### BHOLD Model Generator

- Model roles from parent departments are not inherited

#### BHOLD Analytics

#### BHOLD Attestation

#### BHOLD Reporting

- Custom attribute 'employeeID' not available
- Unable to properly load large data using 3-file set


## Next steps

- [MIM Version release history](version-history.md) includes public release versions history of the Microsoft Identity Manager 2016 SP1.
- [Support team blog build versions](https://blogs.technet.microsoft.com/iamsupport/idmbuildversions/) includes public release versions of the Identity products, including service packs, updates, rollups and hotfixes of MIM 2016, AAD Connect, FIM 2010 R2, and older build types.
