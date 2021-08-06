---
title: Connector Version Release History | Microsoft Docs
description: This topic lists all releases of the Connectors for Forefront Identity Manager (FIM) and Microsoft Identity Manager (MIM)
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft

ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 8/6/2021
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems

---
# Connector Version Release History

Connectors link specific connected data sources to Microsoft Identity Manager (MIM) and Azure AD Connect. (In Forefront Identity Manager, connectors were known as management agents.) Many of the connectors, such as connectors to provision users into Active Directory, are delivered as part of the MIM Synchronization Service installation and the installation package of Azure AD Connect. In addition, more connectors, such as to third-party directory servers, are shipped as a separate download so they can be more frequently updated to add support for connecting to MIM updated versions of third-party target systems.  

> [!NOTE]
> This topic is primarily for MIM Connectors only. Unless explicitly called out below, these Connectors are not supported for install on Azure AD Connect.


This topic lists all versions of the generic connectors package that have been released separately from MIM.  For a list of connectors that are supported with MIM, see [supported connectors in MIM 2016 SP2](../supported-management-agents.md).  Some partners have created their own connectors in this way, and a full list is available in the wiki [FIM 2010 and MIM 2016: Management Agents from Partners](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx).


Related links:

* [Download Latest Connectors](https://go.microsoft.com/fwlink/?LinkId=717495)
* [Graph Connector](~/microsoft-identity-manager-2016-connector-graph.md) reference documentation
* [Generic LDAP Connector](microsoft-identity-manager-2016-connector-genericldap.md) reference documentation
* [Generic SQL Connector](microsoft-identity-manager-2016-connector-genericsql.md) reference documentation
* [Web Services Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) reference documentation
* [PowerShell Connector](microsoft-identity-manager-2016-connector-powershell.md) reference documentation
* [Lotus Domino Connector](microsoft-identity-manager-2016-connector-domino.md) reference documentation
* [SharePoint User Profile Store Connector](https://go.microsoft.com/fwlink/?LinkID=331344) reference documentation

## August 2021
### Updates
- Forefront Identity Manager Connector for Windows Azure Active Directory
  - Existing deployments should migrate to [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect), Azure AD Connect Sync, or the [Microsoft Graph Connector](../microsoft-identity-manager-2016-connector-graph.md).

## 1.1.1431.0 (March 2021)
### Enhancements
- Generic LDAP Connector
  - Added option to hash passwords prior to sending to OpenLDAP
  - Improved Oracle Unified Directory delta import entries processing
- Webservices Connector
  - Added support for TLS 1.2

## 1.1.1381.0 (February 2021)
### Fixed issues
- Graph Connector
  - Fixed an issue with multi-valued string attributes handled incorrectly when sending B2B invitations
- Generic LDAP Connector
  - Improved Oracle Unified Directory changelog entries processing

## 1.1.1347.0 (December 2020)
### Fixed issues
- Graph Connector
  - Fixed an issue with connector incorrectly sending B2B invitations when creating a mail-enabled group or a contact

## 1.1.1346.0 (November 2020)
### Fixed issues
- Graph Connector
  - Fixed an issue with local connector cache corruption causing delta import run failures
  - Fixed an issue with duplicated entries reported by connector during full import run causing discovery errors
  - Fixed an issue with incorrect import of complex data types, e.g. *employeeOrgData*
- Generic SQL Connector
  - Fixed an issue with SQL native authentication failure due to DSN connection string property *TrustedConnection* set to *false* 
- Generic LDAP Connector
  - Fixed an issue with *OpenLDAP* *accessLog* entries processing on delta import causing incorrect group membership changes and other errors

## 1.1.1302.0 (September 2020)
### Fixed issues
- Graph Connector
  - Fixed an issue with schema refresh for */beta* endpoint

## 1.1.1301.0 (August 2020)
### Fixed issues
- Graph Connector
  - Fixed a bug with import failures caused by empty *UserType* attribute value
  - Fixed a bug with delta import failures connector cache not being readable caused by discovery errors
  - Service and proxy reported timeouts can be retried automatically
  - Updated schema discovery for */beta* endpoint 
### Enhancements
- Graph Connector
  - Added support for reading and writing values of custom directory extension attributes
  - Added support for reading *Service Principal* members of groups in the */beta* endpoint 
  - Performance improvements for delta import runs related to schema discovery
  - Graph connector can now invite external member users

  > [!NOTE]
  > If you have been using guest invite in build 1.1.1170.0 of the connector, please update your sync rules with the following logic:

  - Outbound flows
    - A user is invited when exporting the creation of the user, and the export includes a *Mail* attribute but not a *UserPrincipalName attribute*.  If the *UserPrincipalName* is supplied, then a user will be created rather than invited
    - *UserType* attribute only defines whether a user will become a *Member* or a *Guest* (defaults to *Member* if not set)
  - Inbound flows
    - *UserPrincipalName* attribute values of external users are rendered 'as-is'

## 1.1.1170.0 (April 2020)
### Fixed issues
- Generic SQL Connector
   - Fixed a bug with query-based export strategy and multi-valued attributes updates
- Lotus Notes Connector
   - Groups from secondary Notes Address Books are no longer deleted by *AdminP* process. Direct delete operation is used now
- Generic LDAP Connector
   - Fixed a bug with LDAP directory operations attributes, e.g. *pwdUpdateTime*, not visible in schema
### Enhancements
- Graph Connector   
   - UPNs of external guest users are no longer rendered 'as-is', instead they are shown in connector space to look like emails
   - Added support for B2B guest users provisioning

   To invite B2B guest users, you need to:
   - Grant permissions to invite guests to your Azure AD application associated with Graph connector
   - Complete connector configuration section for inviting external users: set Invite Redirect URL (mandatory) and choose whether to send invitation emails
   - Set mandatory attributes in your outbound synchronization rule:
     - "Guest"=>*userType* (initial flow only)
     - external email address=>*userPrincipalName*
     - CustomExpression("CN="+csObjectID+",OBJECT=user")=>*dn* (initial flow only)
     - csObjectID=>*id* (initial flow only)

## 1.1.1130.0 (February 2020)
### Fixed issues
- Graph Connector
   - Export no longer fails when trying to add a member into a group that has been already added
   - 'export_password' virtual attribute support is fixed, no need to set up 'password' attribute flow anymore
   - Fixed export flow of multi-valued string attributes
   - Fixed several bugs affecting delta import
   - Various potential datetime format bugs fixed
- Generic SQL Connector
   - Various UI bugs fixed
   - Fixed incorrect references handling during delta import
   - Fixed a bug with SQL Change Tracking delta import strategy and multi-valued tables to import group membership changes correctly
   - Fixed a bug with attribute values not being cleared on export
   - Fixed a bug with last element of multi-valued reference attribute not being deleted on export
   - Fixed a bug with schema refresh causing reference attributes to be set to strings
   - Fixed a bug with stored procedure parameters values being truncated to 397 bytes
   - Fixed a bug with Oracle tables and views schema detection being case-sensitive
- Lotus Notes Connector
   - Performance improved when importing group members
   - Full import no longer fails with 'Null reference errors'
   - Fixed a bug with Notes mailbox deletion when ACL is set
   - Empty group names no longer cause delta import failures
   - Fixed a bug with non-printing characters left in attributes after string values deletions
- Generic LDAP Connector
   - Delta import no longer shows 'replace' literal value when no value is set in the source changelog
### Enhancements
- Graph Connector
   - Added support for sovereign clouds and ability to configure Login and Graph Resource URLs
   - Unsupported attributes are filtered out and hidden in connector properties after schema discovery

## 4.4.1800.1 (July 2019)
### Enhancements:
- SharePoint User Profile Store Connector
   - Added support for SharePoint Server 2019. The connector is available as a download from the [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=279713) 

## 1.1.953.0 (June 2019)

### Fixed issues:
- Generic LDAP Connector
   - Delta import no longer fails if Changes field is empty in Oracle Internet Directory
- Generic SQL Connector
   - Fixed an issue with custom SQL query import strategy using StartIndex and EndIndex parameters resulting in only the first page be imported
   - Fixed an issue with table/view import strategy when reading from MS SQL resulting in only the first page be imported
- Graph Connector:
   - Organizational contacts are now handled correctly, email is no longer missing
   - Fixed an issue with manager attribute import and export operations. Connector no longer fails when manager is empty. Manager value is updated correctly in Azure AD
   - Fixed an issue with delta import of empty values
   - Fixed an issue with connector crashing on delta import when local cache file is corrupted
   - Fixed several issues with incorrect export of empty values or when only attribute value case has changed
   - Connector now handles delete/add operations for the same attribute during export run correctly
   - Fixed several issues with long-running imports and exports when access tokens were expiring during connector run. Connector now renews access tokens when needed without failure
   - Fixed an issue with last member of a group not being deleted
### Enhancements:
- Generic SQL Connector
   - commandTimeout parameter of a data reader is set to match connector timeout. If you have long-running queries, taking more than 30 seconds to complete, you can increase the "Command Timeout" parameter value in the "Connectivity" section
- Graph Connector: 
   - Added multi-threaded group membership full import strategy to improve import performance. Delta import remains single-threaded operation
   - Added support for complex schema types resulting attributes like OnPremisesExtentionAttributes.* being available now
   - Added support for export_password attribute to avoid export-change-not-reimported errors and do not show initial password in the connector space. Behavior is similar to other ECMA2 connectors
   - Added a handler to support HTTP requests throttling. When Azure AD replica receives too many requests from a client, it might respond with Retry-After instruction. Connector will pause and retry instead of failing
   - Delta import profile will no longer start if query filters are defined. If you want to import only specific objects from Azure AD, for example, users having last name that starts with A*, then delta import functionality will be blocked


## 1.1.913.0 \(January 2019\)

### Fixed issues:

* Generic SQL:
    * Fixed a regression bug on the Generic SQL Connector where it was only reading the first 5000 objects.
* Graph Connector:
    * Fixed an issue where the Graph API Connector failed to read/write from a tenant or create a new connector when the beta option is selected on connectivity.

### Enhancements:

* Graph Connector:
    * Set the TLS 1.2 protocol as the default for the Graph connector.
* Generic SQL Connector:
    * Generic SQL Connector has been tested with Oracle 12c and Oracle 18c.

## 1.1.911.0

> [!NOTE]  
> This connector build version is only for use within Azure AD Connect deployments, and is not provided for use in MIM deployments.
> 
> Compared to the previous connector release, it contains no improvements or updates for MIM customers.

-  Updates the non-standard connectors (for example, Generic LDAP Connector and Generic SQL Connector) shipped with Azure AD Connect.  

## 1.1.861.0

### Fixed issues:
* Change titles for all connector setup from 'Forefront to Microsoft'

* Lotus Notes: 
    * Errors in COM with Lotus sometimes return no errors
* Generic LDAP:
    * Gldap Extra symbol for DI with PING directory server
* Generic Web Services:
    * Error on Exporting json parsing
* Generic SQL:
    * Export Binary Attribute
    * Object types cannot be substrings of each other
    * Changes in multi-valued table are not tracked in the operation of "Delta import", if "Delta Strategy" is "Change Tracking"
* Graph Connector(Public Preview)
    * Error on Group Deletes
    * Update User-Agent to http header
    * Connector doesn't validate Client ID and Client Secret
    * Tenant name filed should be trimmed

### Enhancements:
* Lotus Notes:
    *Add the ability to increase timeout through UI
* Graph Connector(Public Preview)
    *Password attribute is filtered on Import to eliminate "Export-not-reimported".
    *Add support of $filter query parameter -Limited to operations with all filters that work in delta query, will also work in the connector
    *Updated to use nextLink directly instead of extracting skipToken for paging detail [here](https://developer.microsoft.com/en-us/graph/docs/concepts/paging)

## 1.1.830.0

### Fixed issues:
* Resolved ConnectorsLog System.Diagnostics.EventLogInternal.InternalWriteEvent(Message: A device attached to the system is not functioning)
* In this release of connectors you will need to update binding redirect from 3.3.0.0-4.1.3.0 to 4.1.4.0 in miiserver.exe.config
* Generic Web Services:
    * Resolved Valid JSON response could not be saved in configuration tool
* Generic SQL:
    * Export always generates only update query for the operation of deleting. Added to generate a delete query
    * The SQL query, which gets objects for the operation of Delta Import,  if 'Delta Strategy' is 'Change Tracking' was fixed. In this implementation known limitation:  Delta Import with 'Change Tracking' mode does not track changes in multi-valued attributes
    * Added possibility to generate a delete query for case, when it is necessary to delete the last value of multivalued attribute and this row does not contain any other data except value, which it is necessary to delete.
    * System.ArgumentException handling when implemented OUTPUT parameters by SP
    * Incorrect query to make the operation of export into field, that has varbinary(max) type
    * Issue with parameterList variable was initialized twice (in the functions ExportAttributes and GetQueryForMultiValue)


## 1.1.649.0 (AADConnect 1.1.649.0)

### Fixed issues:

* Lotus Notes:
  * Filtering custom certifiers option
  * Import of the class ImportOperations fixed the definition of what operations can be run in the 'Views' mode and which in the 'Search' mode.
* Generic LDAP:
  * OpenLDAP Directory uses DN as anchor rather than entryUUI. New option to GLDAP connector, which allows to modify anchor
* Generic SQL:
  * Fixed export into field, that has varbinary(max) type.
  * When adding binary data from a data source to CSEntry object, The DataTypeConversion function failed on zero bytes. Fixed DataTypeConversion function of the CSEntryOperationBase class.




### Enhancements:

* Generic SQL:
  * The ability to configure the mode for execute stored procedure with named
    parameters or not named is added in a configuration window of the Generic
    SQL management agent in the page 'Global Parameters'. In the page
    'Global Parameters', there is check box with the label 'Use named parameters
    to execute a stored procedure', which is responsible for mode for execute
    stored procedure with named parameters or not.
    * Currently, the ability to execute stored procedure with named parameters
    works only for databases IBM DB2 and MSSQL. For databases Oracle and MySQL
    this approach doesn't work: 
      * The SQL syntax of MySQL doesn't support named parameters in stored
        procedures.
      * The ODBC driver for the Oracle doesn't support named parameters for
        named parameters in stored procedures)

## 1.1.604.0 (AADConnect 1.1.614.0)


### Fixed issues:

* Generic Web Services:
  * Fixed an issue preventing a SOAP project from being created when there were two or more endpoints.
* Generic SQL:
  * In the operation of import the GSQL was not converting time correctly, when saved to connector space. The default date and time format for connector space of the GSQL was changed from 'yyyy-MM-dd hh:mm:ssZ' to 'yyyy-MM-dd HH:mm:ssZ'.

## 1.1.551.0 (AADConnect 1.1.553.0)

### Fixed issues:

* Generic Web Services:
  * The WSconfig tool did not convert correctly the Json array from "sample request" for the REST service method. This caused problems with serialization this Json array for the REST request.
  * Web Service Connector Configuration Tool does not support usage of space symbols in JSON attribute names 
    * A Substitution pattern can be added manually to the WSConfigTool.exe.config file, for example,  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > JSONSpaceNamePattern key is required as for export you will recieve the following error: Message: Empty name is not legal. 

* Lotus Notes:
  * If the option **Allow custom certifiers for Organization/Organizational Units** is disabled, then the connector fails during export (Update) After the export flow all attributes are exported to Domino but at the time of export a KeyNotFoundException is returned to Sync. 
    * This happens because the rename operation fails when it tries to change DN (UserName attribute) by changing one of the attributes below:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  * When **Allow custom certifiers for Organization/Organizational Units** option is enabled, but required certifiers are still empty, then KeyNotFoundException occurs.

### Enhancements:

* Generic SQL:
  * **Scenario: redesigned Implemented:** "*" feature
  * **Solution description:** Changed approach for [multi-valued reference attributes handling](microsoft-identity-manager-2016-connector-genericsql.md).


### Fixed issues:

* Generic Web Services:
  * Can't import Server configuration if WebService Connector is present
  * WebService Connector is not working with multiple  Web Services

* Generic SQL:
  * No object types are listed for single value referenced attribute
  * Delta import on Change Tracking strategy deletes object when value is removed from multi-value table
  * OverflowException in GSQL connector with DB2 on AS/400

Lotus:
  * Added option to enable\disable searching OUs before opening GlobalParameters page

## 1.1.443.0

Released: 2017 March

### Enhancements

* Generic SQL:</br>
  **Scenario Symptoms:**  It is a well-known limitation with the SQL Connector where we only allow a reference to one object type and require cross reference with members. </br>
  **Solution description:** In the processing step for references were "*" option is chosen, ALL combinations of object types will be returned back to the sync engine.

> [!Important]
> - This will create many placeholders
> - It is required to make sure the naming is unique cross object types.


* Generic LDAP:</br>
  **Scenario:**
  When only few containers are selected in specific partition, then the search still will be done in whole partition. Specific will be filtered by Synchronization
  Service, but not by MA, that might cause performance degradation. </br>

  **Solution description:** Changed GLDAP connector's code to make it possible go through all containers and search objects in each of them, instead of searching in the whole partition.


* Lotus Domino:

  **Scenario:** Domino mail deletion support for a person removal during an export. </br>
  **Solution:** Configurable mail deletion support for a person removal during an export.

### Fixed issues:
* Generic Web Services:
  * When changing the service URL in Default SAP WSconfig projects through WebService Configuration Tool then the following error happens:
  Could not find a part of the path

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Generic LDAP:
  * GLDAP Connector does not see all attributes in AD LDS
  * Wizard breaks when no UPN attributes are detected from the LDAP directory schema
  * Delta Imports Failing with discovery errors not present during full import, when "objectclass" attribute is not selected
  * A "Configure Partitions and Hierarchies" configuration page, doesn't show any objects which type is equal to the partition for Novel servers in the Generic  
  LDAP MA. They showed only objects from RootDSE partition.


* Generic SQL:
  * Fix for Generic SQL watermark Delta Import multivalued attribute not imported bug
  * When exporting deleted\added values of multivalued attribute, they are not deleted\added in data source.  


* Lotus Notes:
  * A specific field "Full Name" is shown in the metaverse correctly however when exporting to Notes the value for the attribute is Null or Empty.
  * Fix for duplicate Certifier error
  * When the Object without any data is selected on the Lotus Domino Connector with other objects then we receive the Discovery error while performing Full-Import.
  * When Delta Import is being running on the Lotus Domino Connector, at the end of that run, the Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service sometimes returns an Application Error.
  * Group membership overall works fine and is maintained, except when running the export to try to remove a user from membership it shows as successful with an update, but the user doesn't actually get removed from membership in Lotus Notes.
  * An opportunity to choose mode of export as "Append Item at bottom" was added in configuration GUI of Lotus MA to append new items at bottom during the export for multi-valued attributes.
  * Connector will add the needed logic to delete the file from the Mail Folder and ID Vault.
  * Delete membership not working for cross NAB member.
  * Values should be successfully deleted from multi-valued attribute

## 1.1.117.0
Released: 2016 March

**New Connector**  
Initial release of the [Generic SQL Connector](microsoft-identity-manager-2016-connector-genericsql.md).

**New features:**

* Generic LDAP Connector:
  * Added support for delta import with Isode.
* Web Services Connector:
  * Updated the csEntryChangeResult activity and setImportErrorCode activity to allow object level errors to be returned back to the sync engine.
  * Updated the SAP6 and SAP6User templates to use the new object level error functionality.
* Lotus Domino Connector:
  * For export, you need one certifier per address book. You can now use the same password for all certifiers to make the management easier.

**Fixed issues:**

* Generic LDAP Connector:
  * For IBM Tivoli DS, some reference attributes were not detected correctly.
  * For Open LDAP during a delta import, whitespaces at the beginning and end of strings were truncated.
  * For Novell and NetIQ, an export that moved an object between OUs/containers and at the same time renamed the object failed.
* Web Services Connector:
  * If the web service had multiple end-points for same binding, then the Connector did not correctly discover these end-points.
* Lotus Domino Connector:
  * An export of the fullName attribute to a mail-in database did not work.
  * An export which both added and removed member from a group only exported the added members.
  * If a Notes Document is invalid (the attribute isValid set to false), then the Connector fails.

## Older releases
Before March 2016, the Connectors were released as support topics.

**Generic LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 September
* [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 March
* [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 January
* [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 September
* [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 March

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 September

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 September

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 September
* [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 March
* [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 August
* [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014 February  
* [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 October
* [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 August

## Troubleshooting 

> [!NOTE]
> When updating Microsoft Identity Manager or AADConnect with use of any of ECMA2 connectors. 

You must refresh the connector definition upon upgrade to match or you will receive the following error in the application event log start to report warning ID 6947: "Assembly version in AAD Connector configuration ("X.X.XXX.X") is earlier than the actual version ("X.X.XXX.X") of "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

To refresh the definition:
* Open the Properties for the Connector instance
* Click the Connection / Connect to tab
  * Enter the password for the Connector account
* Click each of the property tabs, in turn
  * If this Connector type has a Partitions tab, with a Refresh button, click the Refresh button while on that tab
* After all property tabs have been accessed, click the OK button to save the changes.

## Other connectors

In addition to the connectors listed above, connectors for SharePoint, and a now-deprecated connector for Windows Azure Active Directory, were also distributed separately from MIM.

### SharePoint User Profile

The Forefront Identity Manager Connector for SharePoint User Profile Store helps you synchronize identity information to the User Profile Store in SharePoint 2013 and SharePoint 2016.   Version 4.3.2430.0 of this connector, published 12/19/2016 can be downloaded from the [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41164).

More information on this connector can be found at the 
[hotfix rollup](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man)
and instructions on how to [Use a sample MIM solution in SharePoint Server 2016](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### Forefront Identity Manager Connector for Windows Azure Active Directory (deprecated connector)

The Azure AD Connector for FIM was an early technology for synchronizing identity information to Azure Active Directory. The Azure AD Connector for FIM, version 1.0.6635.0069 from February 19, 2014, is at feature freeze and deprecated. The solution of using FIM and the Azure AD Connector has been superseded.  Existing deployments should migrate to [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect), Azure AD Connect Sync, or the [Microsoft Graph Connector](../microsoft-identity-manager-2016-connector-graph.md). Do not begin a new deployment using the Forefront Identity Manager Connector for Windows Azure AD.

## Next steps

* Learn more about the [Graph Connector](~/microsoft-identity-manager-2016-connector-graph.md).
* Learn more about the [Generic LDAP Connector](microsoft-identity-manager-2016-connector-genericldap.md).
* Learn more about the [Generic SQL Connector](microsoft-identity-manager-2016-connector-genericsql.md).
* Learn more about the [Web Services Connector](microsoft-identity-manager-2016-ma-ws.md).
* Learn more about the [PowerShell Connector](microsoft-identity-manager-2016-connector-powershell.md).
* Learn more about the [Lotus Domino Connector](microsoft-identity-manager-2016-connector-domino.md).
