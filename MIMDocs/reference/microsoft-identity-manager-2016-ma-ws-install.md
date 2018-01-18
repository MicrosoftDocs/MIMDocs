---
# required metadata

title: MIM Install the Web Service Cofiguration Tool | Microsoft Docs
description: This article covers the steps to install the Web Service Configuration Tool.
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 11/27/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
---

# Install the Web Service Configuration Tool

The Web Service Connector and default projects are available from the [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

The **Web Service Connector MSI** exposes two features:

- Web Service Connector Runtime: Installs the core Connector, the Connector dependencies, and the packaged Connector.
- Web Service Configuration Tool: Installs the Web Service Configuration Tool.

![Installation wizard connector options](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

The configuration tool can be installed without having the Synchronization
Service installed. This allows configuration on a separate computer.

## Default Projects

Additional default projects are shipped with the Web Services Connector. These are available as self-extract EXE files. You may download web service Connector project as appropriate to your requirement.

After the installation is complete, the different components with their binaries are installed at below folder location on your system.

| Contents | Location |
|---|---|
| Web Service Connector Runtime           | %Program Files%\\Microsoft Forefront Identity Management\\2010\\Synchronization&nbsp;Service\\Extensions |
| Web Service Connector Project           | %Program Files%\\Microsoft Forefront Identity Management\\2010\\Synchronization&nbsp;Service\\Extensions |
| Packaged Connector                      | %Program Files%\\Microsoft Forefront Identity Management\\2010\\Synchronization&nbsp;Service\\UIShell\\XMLs\\PackagedMAs |
| Web Service Configuration tool          | %Program Files%\\Microsoft Forefront Identity Management\\2010\\Synchronization&nbsp;Service\\UIShell\\Web&nbsp;Service&nbsp;Configuration <br/>**Note**: This is the default install location. You can change this location during the installation. |
| Web Service Project file                | User can select any target folder to extract this file into, but the extracted project file (.WsConfig) is visible to the FIM Sync UI only when the project file is extracted to the FIM **Extensions** folder. The extracted project file is visible to the Web Service Configuration tool in any location. |


## Additional permissions

Project file can be saved and opened from any location (with the appropriate access privileges of its executor); however, only project files that are saved to the `Synchronization Service\Extension` folder are able to be selected in the Web Service connector wizard accessed through FIM Sync UI.

The user running the Web Service Configuration Tool requires the following privileges:

- Read/Write permissions to the Synchronization Service Extension folder.
- Read access to the registry key **HKLM\\System\\CurrentControlSet\\Services\\FIMSynchronizationService\\Parameters**.
