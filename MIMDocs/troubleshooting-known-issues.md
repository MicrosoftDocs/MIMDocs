---
# required metadata

title: Known issues in MIM 2016
description: Learn about known issues MIM 2016.
keywords:
author: henrymbuguakiarie
ms.author: henrymbugua
ms.date: 04/08/2025
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---


# Known issues: Microsoft Identity Manager (MIM) 2016

This article addresses a known issue affecting the MIM 2016 Portal after installation. It helps users identify the This article documents a known issue affecting Microsoft Identity Manager (MIM) 2016. It helps users recognize the problem, understand its underlying cause, and apply a validated solution where available.

## Issue: MIM Portal fails after SharePoint Security Updates KB5002768 and KB5002754

After applying SharePoint Server 2016 security updates KB5002768 and KB5002754, users may experience unresponsive or partially broken behavior in the MIM 2016 SP2 Portal. The Microsoft MIM Support team has received multiple reports of this issue.

These updates address the following vulnerabilities:

- CVE-2025-53770 – Remote Code Execution Vulnerability
- CVE-2025-53771 – Spoofing Vulnerability

### Troubleshooting steps

To restore MIM Portal functionality, run the following PowerShell script using the SharePoint Management Shell on the server hosting the MIM Portal:

```powershell
$f = Get-SPFarm
$f.AddGenericAllowedListValue("WebPartSupportedBoundPropertyNames", "data-title-text")
$f.AddGenericAllowedListValue("WebPartSupportedBoundPropertyNames", "data-link-to-tab-text")
$f.update()
iisreset
```

### Possible causes

The issue stems from changes introduced by the security updates that affect how custom web parts render in the MIM Portal. These changes block certain properties unless explicitly allowed.

## Related content

- [ASPX file cannot be displayed when you create a custom web part (KB5030804)](https://support.microsoft.com/en-us/topic/aspx-file-cannot-be-displayed-when-you-create-a-custom-web-part-kb5030804-4d8e1e49-dfc9-4261-9d67-cb62ad20e332)
