---
# required metadata

title: BHOLD access management connector installation | Microsoft Docs
description: The BHOLD connector module supports initial and ongoing synchronization of data
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 01/27/2023
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid:


---
# Access Management Connector Installation

The BHOLD Suite Access Management Connector module supports both initial and ongoing synchronization of data into BHOLD. The Access Management Connector works with the Microsoft Identity Manager (MIM) Synchronization Service to move data among the BHOLD Core database, the FIM 2010 metaverse, and target applications and identity stores. After installing the Access Management Connector module, you will be able to create FIM Management Agents that control data flow between BHOLD and MIM.

## Access Management Connector software requirements

Before you can install the Access Management Connector module, you must install Microsoft .NET Framework 4. For more information about .NET Framework 4 and installation instructions, see the [Microsoft .NET home page](https://www.microsoft.com/net).
You must install the Access Management Connector on a computer running the FIM Synchronization Service of MIM.

## Access Management Connector setup

To install the Access Management Control module, log on as a member of the
Domain Admins group, download the following file and run it as administrator on
the server that you intend to install the BHOLD access management connector module on:

- AccessManagementConnector.msi

To run the program file as an administrator, right-click the file and then click
**Run as administrator**.

## Next steps


- [BHOLD installation guide](bhold-installation-guide.md)
- [BHOLD developer reference](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD version history](../reference/version-bhold-history.md)
