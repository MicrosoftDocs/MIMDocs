---
# required metadata

title: Management agents | Microsoft Identity Manager
description: Discover the management agents that help you connect data sources to MIM. 
keywords:
author: kgremban
manager: femila
ms.date: 06/23/2016
ms.topic: article
ms.prod: identity-manager-2015
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

# Supported management agents in MIM 2016

Management agents link specific connected data sources to Microsoft Identity Manager (MIM). A management agent is responsible for moving data from a connected data source to MIM. When data in MIM is modified, the management agent can also export the data to the connected data source to keep it synchronized with MIM. Generally, there is at least one management agent for each connected directory.

There are two types of management agents:
- **Call-based management agents** use an API function call to a data source to import or export data.

- **File-based management agents** use a text file to import data from and export data to a connected data source.

This article covers the management agents that are included in MIM, but the Management Agent for Extensible Connectivity 2.0 makes it possible to connect with even more data sources. Some partners have created their own management agents in this way, and a full list is available in the wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## Call-based management agents

| Name | Supported versions of the connected data source |
| ---- | ----------------------------------------------- |
| Management Agent for Active Directory Domain Services | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Management Agent for Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Management Agent for Active Directory Global Address List (GAL) | Active Directory Global Address List (GAL) – Exchange 2000, 2003, 2007, 2010, 2013 |
| Management Agent for FIM Certificate Management | Forefront Identity Manager 2010 Certificate Management |
| Management Agent for Extensible Connectivity 2.0 | Any call-based data source |
| Management Agent for FIM Service | Forefront Identity Manager 2010 R2 |
| Management Agent for IBM DB2 Universal Database | IBM DB2 version 9.1, 9.5 or 9.7; IBM DB2 OLEDB v9.5 FP5 or v9.7 FP1 |
| Management Agent for IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Management Agent for Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 and 8.8.6 |
| Management Agent for Oracle Database | Oracle Database 10g or 11g; 64-bit client |
| Management Agent for Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
|Management Agent for Oracle (previously Sun and Netscape Directory Servers) | Sun Directory Server 6.x, 7.x and Oracle 11 |
| [Windows PowerShell Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 or better |
| [Windows Azure Active Directory Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Windows Azure Active Directory |
| [Generic LDAP Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3 server (RFC 4510 compliant) |
| [Connector for Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.0.x or v8.5.x |
| [Connector for SAP HCM](https://www.microsoft.com/en-us/download/details.aspx?id=51495) |  SAP ECC 5.0 or 6.0 |
| [SharePoint Services Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint server 2013 with User Profile service application (UPA) |
| [Connector for Oracle eBusiness Suite](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | Oracle E-Business Suite 12.1.3 |
| [Connector for Oracle PeopleSoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | Oracle PeopleSoft 9.1 |
| [Connector for Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 or 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |


## File-based management agents

| Name | Supported versions of the connected data source |
| ---- | ----------------------------------------------- |
| Management Agent for Attribute-Value Pair text file | Attribute-value pair text files |
| Management Agent for Delimited text file | Delimited text files |
| Management Agent for Directory Services Mark-up Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Management Agent for Extensible Connectivity 2.0 | Any file-based data source |
| Management Agent for Fixed-Width text file | Fixed-width text files |
| Management Agent for LDAP Data Interchange Format (LDIF) | LDAP Data Interchange Format (LDIF) |
