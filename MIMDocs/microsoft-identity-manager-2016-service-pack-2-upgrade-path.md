---
# required metadata

title: Upgrade from FIM 2010 R2 and MIM 2016 to Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: Learn how to upgrade your FIM 2010 R2 or MIM 2016 components, and then install the components that are new in MIM 2016 SP2.
keywords:
author: EugeneSergeev
ms.author: esergeev
manager: amycolannino
ms.date: 09/16/2019
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: markwahl-msft
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM 2016 SP2 upgrade  from Forefront Identity Manager or Microsoft Identity Manager

Organizations can upgrade to Microsoft Identity Manager 2016 SP2 from earlier versions of Microsoft Identity Manager or Forefront Identity Manager.  Each section of this article covers a supported upgrade path.

There are several upgrade options available. If you're already running MIM 2016, and do not need to refresh the underlying platform (Windows Server, SQL, SharePoint, SCSM DW) or run MIM services using group managed service accounts, and you do not use MIM Language Packs then the easiest option will be in-place upgrade / hotfix (.msp) installation. Otherwise, the full installation is recommended.

> [!IMPORTANT]
> Check updated [Software prerequisites](prepare-server-ws2016.md#software-prerequisites) section before installing MIM 2016 SP2

## Upgrade from FIM 2010 R2 SP1 or later FIM builds

> [!NOTE]
> The minimal supported version of Forefront Identity Manager that can be directly upgraded to MIM 2016 SP2 is FIM 2010 R2 SP1 (build 4.1.3419.0). Direct upgrade to MIM 2016 SP2 from earlier versions of FIM is not supported. If you're running FIM builds earlier than 4.1.3419.0 then you have to upgrade to FIM 2010 R2 SP1 prior to upgrading to MIM 2016 SP2.

1. **Option 1: Full installation using existing databases**
    1. Make a backup copy of your FIMSynchronizationService and FIMService databases.
    1. Export all FIM Service RCDC objects and RCDC resource strings you made changes to.
    1. Export Synchronization Service encryption keys.
    1. Backup miisserver.exe.config, Synchronization Server 'Extensions' folder, Microsoft.ResourceManagement.Service.exe.config as MIM installer might overwrite custom changes made to config files.
    1. Uninstall **all** FIM components, including Language Packs (to be uninstalled first).
    1. *Optional:* Move FIM databases to another SQL server. It is recommended to create SQL aliases on MIM servers and use these aliases instead of real SQL server name to ease MIM database migration in future.
    1. Install MIM 2016 SP2 from the .iso installation media on the same or on another server following [MIM deployment guide](microsoft-identity-manager-deploy.md), choose to use existing databases when prompted, provide previously exported Synchronization Service encryption keys.
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .NET redirects or any custom sections you added.
    1. Install MIM 2016 SP2 Language Packs if needed.
    1. Resubmit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade FIM Add-ons and Password Reset clients, provide new MIM Service server name if MIM Service server name changed.
    
## Upgrade from previous MIM 2016 builds
1. Make a backup copy of your FIMSynchronizationService and FIMService databases.
1. Export all custom FIM Service localizations made to RCDC objects and RCDC resource strings.
1. Export Synchronization Service encryption keys.
1. Backup miisserver.exe.config, Synchronization Server 'Extensions' folder, Microsoft.ResourceManagement.Service.exe.config as MIM installer might overwrite custom changes made to config files.
1. Uninstall MIM Language Packs if used.
1. Stop MIM services.
1. **Option 1: In-place upgrade - hotfix installation**
    1. Apply MIM 2016 SP2 Synchronization Service [hotfix](https://www.microsoft.com/download/details.aspx?id=100412)
    1. Apply MIM 2016 SP2 Service [hotfix](https://www.microsoft.com/download/details.aspx?id=100412)
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .NET redirects or any custom sections that must be added.
    1. Install MIM 2016 SP2 Language Packs if needed.
    1. Resubmit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade MIM 2016 Add-ons and Password Reset clients.
1. **Option 2: Full installation using existing databases**
    1. Uninstall **all** MIM components.
    1. *Optional:* Move FIM databases to another SQL server. It is recommended to create SQL aliases on MIM servers and use these aliases instead of real SQL server name to ease MIM database migration in future.
    1. Install MIM 2016 SP2 from the .iso installation media on the same or on another server following [MIM deployment guide](microsoft-identity-manager-deploy.md), choose to use existing databases when prompted, provide previously exported Synchronization Service encryption keys.
    1. Review miisserver.exe.config and Microsoft.ResourceManagement.Service.exe.config files for missing .NET redirects or any custom sections that must be added.
    1. Install MIM 2016 SP2 Language Packs if needed.
    1. Resubmit customizations to RCDC objects and RCDC resource strings and localizations if needed.
    1. Upgrade MIM 2016 Add-ons and Password Reset clients, provide new MIM Service server name if MIM Service server name changed.

> [!NOTE]
> Language Packs updates after MIM 2016 SP2 will be distributed as hotfixes (.msp files), eliminating the need to uninstall/reinstall Language Packs.

More detailed information about the upgrade and databases backup procedures could be found in the [Upgrade to FIM 2010 R2](/previous-versions/mim/jj134291%28v%3dws.10%29) article, applicable to any FIM or MIM upgrade process.
