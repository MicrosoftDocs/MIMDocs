---
# required metadata

title: Supported connectors | Microsoft Docs
description: Use connectors to manage data transfer between MIM and your directories.
keywords:
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163


# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Connect to your directories

Connectors link specific connected data sources to Microsoft Identity Manager (MIM). A connector moves data from a connected data source to MIM. When data in MIM is modified, the connector can also export the data to the connected data source to keep it synchronized with MIM. Generally, there is at least one connector for each connected directory.

In Forefront Identity Manager, connectors were known as management agents. That term is still used in some articles or parts of the product, but know that both terms refer to the same concept.

This article covers the connectors that are included in MIM, but the connector for Extensible Connectivity 2.0 makes it possible to connect with even more data sources. Some partners have created their own connectors in this way, and a full list is available in the wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## Supported connectors in MIM 2016

| Name | Supported versions of the connected data source |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory Global Address List (GAL) – Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Any call-based or file-based data source |
| MIM Service | Microsoft Docs 2016 |
| IBM DB2 Universal Database | IBM DB2 version 9.1, 9.5 or 9.7; IBM DB2 OLEDB v9.5 FP5 or v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 and 8.8.6 |
| Oracle Database | Oracle Database 10g or 11g; 64-bit client |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Oracle (previously Sun and Netscape) Directory Servers | Sun Directory Server 6.x, 7.x and Oracle 11 |
| [Windows PowerShell Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 or better |
| [Microsoft Azure Active Directory Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Generic LDAP Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3 server (RFC 4510 compliant) |
| [Connector for Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.0.x or v8.5.x |
| [SharePoint Services Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013, 2016, or 2019 with User Profile Service Application (UPA) |
| [Connector for Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 or 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| Attribute-Value Pair text file | Attribute-value pair text files |
| Delimited text file | Delimited text files |
| Directory Services Mark-up Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Fixed-Width text file | Fixed-width text files |
| LDAP Data Interchange Format (LDIF) | LDAP Data Interchange Format (LDIF) |

## Related topics

[Management agents in FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
