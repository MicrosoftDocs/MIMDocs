---
# required metadata

title: BHOLD access management connector installation | Microsoft Docs
description: The BHOLD connector module supports initial and ongoing synchronization of data
keywords:
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid:


---
# Access Management Connector Installation

The BHOLD Suite Access Management Connector module supports both initial and ongoing synchronization of data into BHOLD. The Access Management Connector works with the Microsoft Identity Manager (MIM) Synchronization Service to move data among the BHOLD Core database, the FIM 2010 metaverse, and target applications and identity stores. After installing the Access Management Connector module, you will be able to create FIM Management Agents that control data flow between BHOLD and MIM.

## Access Management Connector software requirements

Before you can install the Access Management Connector module, you must install Microsoft .NET Framework 4. For more information about .NET Framework 4 and installation instructions, see the [Microsoft .NET home page](http://www.microsoft.com/net).
You must install the Access Management Connector on a computer running the FIM Synchronization Service of MIM.

## Access Management Connector setup

To install the Access Management Control module, log on as a member of the
Domain Admins group, download the following file and run it as administrator on
the server that you intend to install the BHOLD FIM Integration module on:

- AccessManagementConnector.msi

To run the program file as an administrator, right-click the file and then click
**Run as administrator**.

## Next steps

- [BHOLD FIM integration installation](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) To enable end-user self-service of roles, you can install the BHOLD FIM Integration module
- [BHOLD installation guide](bhold-installation-guide.md)
- [BHOLD developer reference](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD version history](../reference/version-bhold-history.md)
