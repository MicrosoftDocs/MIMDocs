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

These updates were released to address the following critical vulnerabilities:

- [CVE-2025-53770 – Microsoft SharePoint Server Remote Code Execution Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-53770)
- [CVE-2025-53771 – Microsoft SharePoint Server Spoofing Vulnerability](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2025-53771)

### Troubleshooting steps

To restore MIM Portal functionality, run the following PowerShell script using the SharePoint Management Shell on the server hosting the MIM Portal:

```powershell
$f = Get-SPFarm $f.AddGenericAllowedListValue("WebPartSupportedBoundPropertyNames", "data-title-text") 
$f.AddGenericAllowedListValue("WebPartSupportedBoundPropertyNames", "data-link-to-tab-text") 
$f.update() iisreset 
```

This workaround aligns with guidance provided in [ASPX file cannot be displayed when you create a custom web part (KB5030804)](https://support.microsoft.com/topic/aspx-file-displayed-custom-web-part-kb5030804-4d8e1e49-dfc9-4261-9d67-cb62ad20e332), which addresses ASPX rendering issues in custom web parts.

### Possible causes

The issue stems from changes introduced by the SharePoint security updates. These changes block certain properties used in custom web parts unless they are explicitly allowed, affecting MIM Portal rendering and responsiveness.

## Related content

- [SharePoint Server 2016 security updates](https://support.microsoft.com/topic/sharepoint-server-2016-security-updates-0f8b1c2a-3d5e-4f7c-bb9d-2c1e0b8f3a4c)
