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

This article documents a known issue affecting Microsoft Identity Manager (MIM) 2016. It helps users recognize the problem, understand the underlying cause, and apply a validated solution where available.

## Issue: MIM Portal fails after SharePoint security updates KB5002768 and KB5002754

The MIM 2016 SP2 Portal may become partially broken or unresponsive after the installation of SharePoint Server 2016 security updates KB5002768 and KB5002754. The Microsoft MIM Support team has received multiple reports confirming this issue.

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

This workaround aligns with guidance provided in [ASPX file can't be displayed when you create a custom web part (KB5030804)](https://support.microsoft.com/topic/aspx-file-displayed-custom-web-part-kb5030804-4d8e1e49-dfc9-4261-9d67-cb62ad20e332), which addresses ASPX rendering issues in custom web parts.

### Possible causes

The issue stems from changes introduced by the SharePoint security updates. These changes block certain properties used in custom web parts unless they're explicitly allowed, affecting MIM Portal rendering and responsiveness.
