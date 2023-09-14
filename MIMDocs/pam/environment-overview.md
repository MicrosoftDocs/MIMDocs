---
# required metadata

title: PAM environment overview | Microsoft Docs
description: Find the required number and configuration of virtual machines to successfully deploy Privileged Access Management
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# MIM PAM test lab environment overview

> [!NOTE]
> The PAM approach provided by MIM is intended to be used in a custom architecture for isolated environments where Internet access is not available, where this configuration is required by regulation, or in high impact isolated environments like offline research laboratories and disconnected operational technology or supervisory control and data acquisition environments. If your Active Directory is part of an Internet-connected environment, see instead [securing privileged access](/security/compass/overview) for more information on where to start.

To set up a test lab of MIM PAM, you can install the software on virtual machines.
Privileged Access Management works with virtual machines (VMs) with separate drives that are connected to each other on a shared network. These virtual machines can be hosted by Windows Server or other operating system platforms.

![PAM servers: relationships and supported platforms - diagram](media/pam-test-lab-architecture.png)

You need a minimum of three virtual machines.  If you don't already have an AD domain for PAM to manage, you need one additional VM to act as a CORP domain controller.  If you wish to configure the PRIV software for high availability, you need two additional VMs.

The drives where the VM disk images will be stored need at least 120 GB of free disk space.  If you plan to deploy for high availability, make sure that the disk subsystem meets the requirements for SQL shared storage.  The shared storage can be in the form of Windows Server Failover Clustering cluster disks, disks on a Storage Area Network (SAN), or file shares on an SMB server.

> [!IMPORTANT]
> Storage must be dedicated to the bastion environment. Sharing storage with other workloads outside of the bastion environment is not recommended as it could jeopardize the integrity of the bastion environment.

## Next steps

- [Privileged Access Management for Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) is an overview of PAM and how it works.
- [Understand the components of PAM](principles-of-operation.md) is an overview of the various components of PAM.
