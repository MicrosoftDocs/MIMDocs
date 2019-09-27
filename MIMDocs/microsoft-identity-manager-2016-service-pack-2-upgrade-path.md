---
# required metadata

title: Upgrade from FIM 2010 R2 and MIM 2016 SP2 to Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: Learn how to upgrade your FIM 2010 R2 or MIM 2016 SP2 components, and then install the components that are new in MIM 2016.
keywords:
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Supported MIM 2016 SP2 upgrade path

> [!NOTE]
The minimal supported version of Forefront Identity Manager that can be directly upgraded to MIM 2016 SP2 is FIM 2010 R2 SP1 (build 4.1.3419.0). Direct upgrade to MIM 2016 SP2 from earlier versions of FIM is not supported. If you're running FIM builds earlier than 4.1.3419.0 then you have to upgrade to FIM 2010 R2 SP1 prior to upgrading to MIM 2016 SP2.

> [!IMPORTANT] There are several upgrade options available. If you're already running MIM 2016 and do not need to refresh the underlying platform (Windows Server, SQL, SharePoint, SCSM DW) or run MIM services using group managed service accounts, then the easiest option will be in-place upgrade / hotfix (.msp) installation. Otherwise, the full installation is recommended.

## Upgrade from FIM 2010 R2 SP1 or later FIM builds

1. Make a backup copy of your FIMSynchronizationService and FIMService databases.
1. Export all FIM Service RCDC objects and RCDC resource strings you made changes to.
1. Export Synchronization Service encryption keys.
1. Backup miisserver.exe.config, Synchronization Server 'Extensions' folder, Microsoft.ResourceManagement.Service.exe.config as MIM installer might overwrite custom changes made to config files.
1. Uninstall FIM Language Packs if used.
1. Stop FIM services.
1. **Option 1: in-place upgrade**
    1. Install MIM 2016 SP2 from the .iso installation media following [MIM deployment guide](microsoft-identity-manager-deploy.md), the installer will detect the existing FIM version and suggest an upgrade. Click on the Update button to proceed.
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .net redirects or any custom sections you added.
    1. Install MIM 2016 SP2 "Service and Portal" and "Add-ins" Language Packs if needed.
    1. Re-submit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade FIM Add-ons and Password Reset clients to MIM 2016 SP2 clients.
 1. **Option 2: side-by-side full installation on another server**
    1. Copy FIM databases to another SQL server if needed. It is recommended to create SQL aliases on MIM servers and use these aliases instead of the real SQL server name to ease MIM DBs migration in the future.
    1. Install MIM 2016 SP2 from the .iso installation media following [MIM deployment guide](microsoft-identity-manager-deploy.md) on another server, choose to use existing databases when prompted, provide previously exported Synchronization Service encryption keys.
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .net redirects or any custom sections that must be added.
    1. Install MIM 2016 SP2 "Service and Portal" and "Add-ins" Language Packs if needed.
    1. Re-submit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade FIM Add-ons and Password Reset clients, provide new MIM Service server name if MIM Service server name changed.

## Upgrade from previous MIM 2016 builds
1. Make a backup copy of your FIMSynchronizationService and FIMService databases.
1. Export all custom FIM Service localizations made to RCDC objects and RCDC resource strings.
1. Export Synchronization Service encryption keys.
1. Backup miisserver.exe.config, Synchronization Server 'Extensions' folder, Microsoft.ResourceManagement.Service.exe.config as MIM installer might overwrite custom changes made to config files.
1. Uninstall MIM Language Packs if used.
1. Stop MIM services.
1. **Option 1: in-place upgrade - [hotfix](https://www.microsoft.com/en-us/download/details.aspx?id=58213) installation**
    1. Apply MIM 2016 SP2 Synchronization Service hotfix
    1. Apply MIM 2016 SP2 Service hotfix
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .net redirects or any custom sections that must be added.
    1. Install MIM 2016 SP2 "Service and Portal" and "Add-ins" Language Packs if needed.
    1. Re-submit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade MIM 2016 Add-ons and Password Reset clients.
1. **Option 2: in-place upgrade - full installation**
    1. Install MIM 2016 SP2 from the .iso installation media following [MIM deployment guide](microsoft-identity-manager-deploy.md), the installer will detect the existing MIM version and suggest an upgrade. Click on the Update button to proceed.
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .net redirects or any custom sections that must be added.
    1. Install MIM 2016 SP2 "Service and Portal" and "Add-ins" Language Packs if needed.
    1. Re-submit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade MIM Add-ons and Password Reset clients.
1. **Option 3: side-by-side full installation on another server**
    1. Copy MIM databases to another SQL server if needed. It is recommended to create SQL aliases on MIM servers and use these aliases instead of the real SQL server name to ease MIM DBs migration in the future.
    1. Install MIM 2016 SP2 from the .iso installation media following [MIM deployment guide](microsoft-identity-manager-deploy.md) on another server, choose to use existing databases when prompted, provide previously exported Synchronization Service encryption keys.
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .net redirects or any custom sections that must be added.
    1. Install MIM 2016 SP2 "Service and Portal" and "Add-ins" Language Packs if needed.
    1. Re-submit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade MIM 2016 Add-ons and Password Reset clients, provide new MIM Service server name if MIM Service server name changed.

> [!NOTE]
Post-SP2 Language Packs updates will be distributed as hotfixes (.msp files), eliminating the need to uninstall/reinstall Language Packs.

More detailed information about the upgrade and databases backup procedures could be found in the [Upgrade to FIM 2010 R2](https://docs.microsoft.com/en-us/previous-versions/mim/jj134291%28v%3dws.10%29) article that is applicable to any FIM or MIM upgrade process . 