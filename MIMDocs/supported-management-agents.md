---
# required metadata

title: Supported connectors
description: Use connectors to manage data transfer between MIM and your connected data sources.
keywords:

ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: amycolannino
editor: ''
reviewer: markwahl-msft

ms.topic: article
ms.date: 03/20/2024
ms.service: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems

---

# Connect to your directories

Connectors link specific connected data sources to Microsoft Identity Manager (MIM). A connector moves data from a connected data source to MIM. When data in MIM is modified, the connector can also export the data to the connected data source to keep it synchronized with MIM. Generally, there is at least one connector for each connected directory.

In *Microsoft Identity Manager (MIM)*, formerly known as *Forefront Identity Manager (MIM)*, connectors were known as *management agents*. That term is still used in some articles or parts of the product, but know that both terms refer to the same concept.

This article covers the connectors that are included & supported in MIM, but the connector for [Extensible Connectivity 2.0](/previous-versions/windows/desktop/forefront-2010/hh859557(v=vs.100)) makes it possible to connect with even more data sources. Some partners have created their own connectors in this way, and a full list is available in the wiki [FIM 2010: Management Agents from Partners](/microsoft-identity-manager/mim-best-practices).

This table doesn't include the software on which MIM itself is deployed; see the [supported platforms](microsoft-identity-manager-2016-supported-platforms.md) list for more information.

## Supported connectors in MIM 2016 SP2

| Connector name | Supported versions of the connected data source & Technical links |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory in Windows Server 2012 - 2022 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory Global Address List (GAL) in Exchange 2013 - 2019 |
| Extensible Connectivity 2.0 | Any call-based or file-based data source |
| FIM Service | MIM Service. MIM Synchronization Service and MIM Service must be the same version. |
| IBM DB2 Universal Database | IBM DB2 version 9.5 or 9.7; IBM DB2 OLEDB v9.5 FP5 or v9.7 FP1 <br/> Use Generic SQL connector for later versions|
| IBM Directory Server | IBM Tivoli Directory Server 6.x <br/> Use Generic LDAP connector for later versions|
| Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 and 8.8.6 <br/> Use Generic LDAP connector for later versions|
| Oracle Database | Oracle Database 10g or 11g; 64-bit client <br/> Use Generic SQL connector for later versions|
| Microsoft SQL Server | SQL Server 2012 - 2017 <br/> Use Generic SQL connector for later versions or SQL Azure|
| Oracle (previously Sun and Netscape) Directory Servers | Sun Directory Server 6.x, 7.x and Oracle 11<br/> Use Generic LDAP connector for later versions |
| [Windows PowerShell Connector](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 or better |
| [Generic CSV Connector](reference/microsoft-identity-manager-2016-connector-genericcsv.md) | User and Groups in CSV files. Support for pre-and-post processing of identity data using PowerShell scripts either before or after import or export operations. |
| [Generic LDAP Connector](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3 server (RFC 4510 compliant)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector), including 389 Directory Server, Apache Directory Server, IBM Tivoli DS, Isode Directory, NetIQ eDirectory, Novell eDirectory, Open DJ, Open DS, Open LDAP, Oracle Directory Server Enterprise Edition, RadiantOne Virtual Directory Server, Sun One Directory Server |
| [Generic SQL Connector](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [The Connector is supported with all 64-bit ODBC drivers](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) including the following: Microsoft SQL Server & SQL Azure, IBM DB2 10.x, IBM DB2 9.x, Oracle 10 & 11g, Oracle 12c & 18c, MySQL 5.x|
| [Connector for Lotus Domino](reference/microsoft-identity-manager-2016-connector-domino.md) | IBM Domino Release v9.0.x |
| [SharePoint Services Connector UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint server 2013 - 2019 with User Profile service application (UPA) |
| [Connector for Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 or 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 and other SOAP and REST APIs](/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Attribute-Value Pair text file](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Attribute-value pair text files |
| [Delimited text file](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Delimited text files |
| [Directory Services Mark-up Language (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Fixed-Width text file](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Fixed-width text files |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Microsoft Graph Connector](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

The Microsoft Azure Active Directory Connector is no longer supported; use [Microsoft Entra Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect) sync, Microsoft Entra Connect cloud provisioning, or [Microsoft Graph Connector](microsoft-identity-manager-2016-connector-graph.md) instead.

## Related articles

[Management agents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
