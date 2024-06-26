---
title: Generic LDAP Connector
description: This article describes how to configure Microsoft's Generic LDAP Connector.
services: active-directory
documentationcenter: ''
author: billmath
manager: amycolannino
editor: ''
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.reviewer: davidste
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.service: microsoft-identity-manager
ms.date: 03/29/2024
ms.author: billmath
---

# Generic LDAP Connector technical reference
This article describes the Generic LDAP Connector. The article applies to the following products:

* Microsoft Identity Manager 2016 (MIM2016)
* [Microsoft Entra ID](/entra/identity/app-provisioning/on-premises-ldap-connector-configure)

For MIM2016, the Connector is available as a download from the [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=717495).

When referring to IETF RFCs, this document is using the format (RFC [RFC number]/[section in RFC document]), for example (RFC 4512/4.3).
You can find more information at [https://tools.ietf.org/](https://tools.ietf.org/). In the left panel, enter an RFC number in the **Doc fetch** dialog box and test it to make sure it is valid.

> [!NOTE]
> [Microsoft Entra ID](/entra/identity/app-provisioning/user-provisioning) now provides a lightweight agent based solution for provisioning users into an LDAPv3 server, without needing a MIM sync deployment. We recommend using it for outbound user provisioning. [Learn more](/entra/identity/app-provisioning/on-premises-ldap-connector-configure).

## Overview of the Generic LDAP Connector
The Generic LDAP Connector enables you to integrate the synchronization service with an LDAP v3 server.

Certain operations and schema elements, such as those needed to perform delta import, aren't specified in the IETF RFCs. For these operations, only LDAP directories explicitly specified are supported.

For connecting to the directories, we test using the root/admin account.  To use a different account to apply more granular permissions, you may need to review with your LDAP directory team.

The current release of the connector supports these features:

| Feature | Support |
| --- | --- |
| Connected data source |The Connector is supported with all LDAP v3 servers (RFC 4510 compliant), except where called out as unsupported. It has been tested with these directory servers: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory Global Catalog (AD GC)</li><li>389 Directory Server</li><li>Apache Directory Server</li><li>IBM Tivoli DS</li><li>Isode Directory</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Open DJ</li><li>Open DS</li><li>Open LDAP (openldap.org)</li><li>Oracle (previously Sun) Directory Server Enterprise Edition</li><li>RadiantOne Virtual Directory Server (VDS)</li><li>Sun One Directory Server</li><li>Microsoft Active Directory Domain Services (AD DS)</li><ul><li>For most scenarios, you must use the built-in Active Directory Connector instead as some features may not work</li></ul>**Notable known directories or features not supported:**<li>Microsoft Active Directory Domain Services (AD DS)<ul><li>Password Change Notification Service(PCNS)</li><li>Exchange provisioning</li><li>Delete of Active Sync Devices</li><li>Support for nTDescurityDescriptor</li></ul></li><li>Oracle Internet Directory (OID)</li> |
| Scenarios |<li>Object Lifecycle Management</li><li>Group Management</li><li>Password Management</li> |
| Operations |The following operations are supported on all LDAP directories: <li>Full Import</li><li>Export</li>The following operations are only supported on specified directories:<li>Delta import</li><li>Set Password, Change Password</li> |
| Schema |<li>Schema is detected from the LDAP schema (RFC3673 and RFC4512/4.2)</li><li>Supports structural classes, aux classes, and extensibleObject object class (RFC4512/4.3)</li> |

### Delta import and password management support
Supported Directories for Delta import and Password management:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Supports all operations for delta import
  * Supports Set Password
* Microsoft Active Directory Global Catalog (AD GC)
  * Supports all operations for delta import
  * Supports Set Password
* 389 Directory Server
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* Apache Directory Server
  * Doesn't support delta import since this directory Doesn't have a persistent change log
  * Supports Set Password
* IBM Tivoli DS
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* Isode Directory
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* Novell eDirectory and NetIQ eDirectory
  * Supports Add, Update, and Rename operations for delta import
  * Doesn't support Delete operations for delta import
  * Supports Set Password and Change Password
* Open DJ
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* Open DS
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* Open LDAP (openldap.org)
  * Supports all operations for delta import
  * Supports Set Password
  * Doesn't support change password
* Oracle (previously Sun) Directory Server Enterprise Edition
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* RadiantOne Virtual Directory Server (VDS)
  * Must be using version 7.1.1 or higher
  * Supports all operations for delta import
  * Supports Set Password and Change Password
* Sun One Directory Server
  * Supports all operations for delta import
  * Supports Set Password and Change Password

### Prerequisites
Before you use the Connector, make sure you have the following on the synchronization server:

* Microsoft .NET 4.6.2 Framework or later

Deploying this connector may require changes to the configuration of the directory server as well as configuration changes to MIM.  For deployments involving integrating MIM with a third-party directory server in a production environment, we recommend customers work with their directory server vendor, or a deployment partner for help, guidance, and support for this integration.

### Detecting the LDAP server
The Connector relies upon various techniques to detect and identify the LDAP server. The Connector uses the Root DSE, vendor name/version, and it inspects the schema to find unique objects and attributes known to exist in certain LDAP servers. This data, if found, is used to pre-populate the configuration options in the Connector.

### Connected Data Source permissions
To perform import and export operations on the objects in the connected directory, the connector account must have sufficient permissions. The connector needs write permissions to be able to export, and read permissions to be able to import. Permission configuration is performed within the management experiences of the target directory itself.

### Ports and protocols
The connector uses the port number specified in the configuration, which by default is 389 for LDAP and 636 for LDAPS.

For LDAPS, you must use SSL 3.0 or TLS. SSL 2.0 isn't supported and cannot be activated.

### Required controls and features
The following LDAP controls/features must be available on the LDAP server for the connector to work properly:  
`1.3.6.1.4.1.4203.1.5.3` True/False filters

The True/False filter is frequently not reported as supported by LDAP directories and might show up on the **Global Page** under **Mandatory Features Not Found**. It is used to create **OR** filters in LDAP queries, for example when importing multiple object types. If you can import more than one object type, then your LDAP server supports this feature.

If you use a directory where a unique identifier is the anchor the following feature must also be available (For more information, see the [Configure Anchors](#configure-anchors) section):  
`1.3.6.1.4.1.4203.1.5.1` All operational attributes

If the directory has more objects than what can fit in one call to the directory, then it is recommended to use paging. For paging to work, you need one of the following options:

**Option 1:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**Option 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

If both options are enabled in the connector configuration, pagedResultsControl is used.

`1.2.840.113556.1.4.417` ShowDeletedControl

ShowDeletedControl is only used with the USNChanged delta import method to be able to see deleted objects.

The connector tries to detect the options present on the server. If the options cannot be detected, a warning is present on the Global page in the connector properties. Not all LDAP servers present all controls/features they support and even if this warning is present, the connector might work without issues.

### Delta import
Delta import is only available when a directory that supports it has been detected. The following methods are currently used:

* LDAP Accesslog. See [http://www.openldap.org/doc/admin24/overlays.html#Access Logging](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* LDAP Changelog. See [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* TimeStamp. For Novell/NetIQ eDirectory, the Connector uses last date/time to get created and updated objects. Novell/NetIQ eDirectory doesn't provide an equivalent means to retrieve deleted objects. This option can also be used if no other delta import method is active on the LDAP server. This option isn't able to import deleted objects.
* USNChanged. See: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### Not supported
The following LDAP features aren't supported:

* LDAP referrals between servers (RFC 4511/4.1.10)

## Create a new Connector
To Create a Generic LDAP connector, in **Synchronization Service** select **Management Agent** and **Create**. Select the **Generic LDAP (Microsoft)** Connector.

![MIM Sync UI to Create a new Connector](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### Connectivity
On the Connectivity page, you must specify the Host, Port, and Binding information. Depending on which Binding is selected, additional information might be supplied in the following sections.

![MIM Sync connector configuration Connectivity page](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* The Connection Timeout setting is only used for the first connection to the server when detecting the schema.
* If Binding is Anonymous, then neither username / password nor certificate are used.
* For other bindings, enter information either in username / password or select a certificate.
* If you are using Kerberos to authenticate, then also provide the Realm/Domain of the user.

The **attribute aliases** text box is used for attributes defined in the schema with RFC4522 syntax. These attributes cannot be detected during schema detection and the Connector needs those attributes to be configured separately. For example the following string must be entered in the attribute aliases box to correctly identify the userCertificate attribute as a binary attribute:

`userCertificate;binary`

The following table is an example for how this configuration could look like:

![MIM Sync connector configuration Connectivity page with attributes](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Select the **include operational attributes in schema** checkbox to also include attributes created by the server. These include attributes such as when the object was created and last update time.

Select **Include extensible attributes in schema** if extensible objects (RFC4512/4.3) are used and enabling this option allows every attribute to be used on all object. Selecting this option makes the schema very large so unless the connected directory is using this feature the recommendation is to keep the option unselected.

### Global Parameters
On the Global Parameters page, you configure the DN to the delta change log and additional LDAP features. The page is pre-populated with the information provided by the LDAP server.

![MIM Sync connector configuration global parameters page](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

The top section shows information provided by the server itself, such as the name of the server. The Connector also verifies that the mandatory controls are present in the Root DSE. If these controls aren't listed, a warning is presented. Some LDAP directories do not list all features in the Root DSE and it is possible that the Connector works without issues even if a warning is present.

The **supported controls** checkboxes control the behavior for certain operations:

* With tree delete selected, a hierarchy is deleted with one LDAP call. With tree delete unselected, the connector does a recursive delete if needed.
* With paged results selected, the Connector does a paged import with the size specified on the run steps.
* The VLVControl and SortControl is an alternative to the pagedResultsControl to read data from the LDAP directory.
* If all three options (pagedResultsControl, VLVControl, and SortControl) are unselected then the Connector imports all object in one operation, which might fail if it is a large directory.
* ShowDeletedControl is only used when the Delta import method is USNChanged.

The change log DN is the naming context used by the delta change log, for example **cn=changelog**. This value must be specified to be able to do delta import.

The following table is a list of default change log DNs:

| Directory | Delta change log |
| --- | --- |
| Microsoft AD LDS and AD GC |Automatically detected. USNChanged. |
| Apache Directory Server |Not available. |
| Directory 389 |Change log. Default value to use: **cn=changelog** |
| IBM Tivoli DS |Change log. Default value to use: **cn=changelog** |
| Isode Directory |Change log. Default value to use: **cn=changelog** |
| Novell/NetIQ eDirectory |Not available. TimeStamp. The Connector uses last updated date/time to get added and updated records. |
| Open DJ/DS |Change log.  Default value to use: **cn=changelog** |
| Open LDAP |Access log. Default value to use: **cn=accesslog** |
| Oracle DSEE |Change log. Default value to use: **cn=changelog** |
| RadiantOne VDS |Virtual directory. Depends on the directory connected to VDS. |
| Sun One Directory Server |Change log. Default value to use: **cn=changelog** |

The password attribute is the name of the attribute the Connector should use to set the password in password change and password set operations.
This value is by default set to **userPassword** but can be changed when needed for a particular LDAP system.

In the additional partitions list, it is possible to add additional namespaces not automatically detected. For example, this setting can be used if several servers make up a logical cluster, which should all be imported at the same time. Just as Active Directory can have multiple domains in one forest but all domains share one schema, the same can be simulated by entering the additional namespaces in this box. Each namespace can import from different servers and is further configured on the Configure Partitions and Hierarchies page. Use Ctrl+Enter to get a new line.

### Configure Provisioning Hierarchy
This page is used to map the DN component, for example OU, to the object type that should be provisioned, for example organizationalUnit.

![Provisioning Hierarchy](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

By configuring provisioning hierarchy, you can configure the Connector to automatically create a structure when needed. For example, if there is a namespace dc=contoso,dc=com and a new object cn=Joe, ou=Seattle, c=US, dc=contoso, dc=com is provisioned, then the Connector can create an object of type country for US and an organizationalUnit for Seattle if those aren't already present in the directory.

### Configure Partitions and Hierarchies
On the partitions and hierarchies page, select all namespaces with objects you plan to import and export.

![MIM Sync connector configuration Partitions page](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

For each namespace, it is also possible to configure connectivity settings that would override the values specified on the Connectivity screen. If these values are left to their default blank value, the information from the Connectivity screen is used.

It is also possible to select which containers and OUs the Connector should import from and export to.

When performing a search this is done across all containers in the partition. In cases where there are large numbers of containers this behavior leads to performance degradation.

> [!NOTE]
> Starting in the March 2017 update to the Generic LDAP connector searches can be limited in scope to only the selected containers. This can be done by selecting the checkbox 'Search only in selected containers' as shown in the image below.

![Search only selected containers](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### Configure Anchors
This page always has a preconfigured value and cannot be changed. If the server vendor has been identified, then the anchor might be populated with an immutable attribute, for example the GUID for an object. If it has not been detected or is known to not have an immutable attribute, then the connector uses dn (distinguished name) as the anchor.

![MIM Sync connector configuration anchors page](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


The following table is a list of LDAP servers and the anchor being used:

| Directory | Anchor attribute |
| --- | --- |
| Microsoft AD LDS and AD GC |objectGUID |
| 389 Directory Server |dn |
| Apache Directory |dn |
| IBM Tivoli DS |dn |
| Isode Directory |dn |
| Novell/NetIQ eDirectory |GUID |
| Open DJ/DS |dn |
| Open LDAP |dn |
| Oracle ODSEE |dn |
| RadiantOne VDS |dn |
| Sun One Directory Server |dn |

## Other notes
This section provides information of aspects that are specific to this Connector or for other reasons are important to know.

### Delta import
The delta watermark in Open LDAP is UTC date/time. For this reason, the clocks between FIM Synchronization Service and the Open LDAP must be synchronized. If not, some entries in the delta change log might be omitted.

For Novell eDirectory, the delta import isn't detecting any object deletes. For this reason, it is necessary to run a full import periodically to find all deleted objects.

For directories with a delta change log that is based on date/time, it is highly recommended to run a full import at periodic times. This process allows the sync engine to find and dissimilarities between the LDAP server and what is currently in the connector space.

## Troubleshooting
* For information on how to enable logging to troubleshoot the connector, see the [How to Enable ETW Tracing for Connectors](/archive/technet-wiki/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors).
