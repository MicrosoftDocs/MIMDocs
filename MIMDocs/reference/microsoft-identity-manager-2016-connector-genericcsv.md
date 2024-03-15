---
title: Generic CSV Connector | Microsoft Docs
description: This article describes how to configure Microsoft's Generic CSV Connector.
services: active-directory
documentationcenter: ''
author: erichkarch
manager: esergeev
editor: ''

ms.assetid: f2a1c6b0-3e7d-4e8e-9a8b-5a6d3f2e1b9d
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 03/13/2024
ms.author: erichkarch

---
# Generic CSV Connector technical reference
This article describes the Generic SQL Connector. The article applies to the following  products:

* [Microsoft Entra Connect Provisioning Agent (ECMA2Host)](https://learn.microsoft.com/en-us/entra/identity/app-provisioning/on-premises-application-provisioning-architecture)
* [Microsoft Identity Manager 2016 (MIM2016)](https://learn.microsoft.com/en-us/microsoft-identity-manager)

For MIM2016, the Connector is available as a download from the [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=717495).

> (!) NOTE
> The [Azure AD provisioning](https://learn.microsoft.com/azure/active-directory/app-provisioning/user-provisioning) service now provides a lightweight agent based solution for provisioning users into an SQL database, without a full MIM sync deployment. We recommend evaluating if it meets your needs. [Learn more](https://learn.microsoft.com/azure/active-directory/app-provisioning/on-premises-sql-connector-configure).

## Overview of the Generic CSV Connector
The Generic CSV Connector lets you integrate User and Group identity data maintained in CSV files with Microsoft products, such as the Microsoft Entra Connect Provisioning Agent (ECMA2Host) and Microsoft Identity Manager 2016 (MIM2016).  

It has various features, such as the ability to orchestrate the use of PowerShell to manage identity data before-or-after imports or exports operations. It offers support for multiple datatypes including binary and references, support for qualified-string values, and multivalued attribute types.  

This article describes the features and functions of the Generic CSV Connector, and how to configure it for MIM 2016.  

The following table lists the features that the current release of the connector supports, from a high-level perspective:

| Feature | Details |
| --- | --- |
| Multiple Product Support | Use of this connector is supported with the following Microsoft products: <li>Microsoft Entra Connect Provisioning Agent (ECMA2Host)</li><li>Microsoft Identity Manager 2016 (MIM2016)</li> |
| CSV Files Supported | This connector supports the management of user (required) and groups (optional), through the configuration of up to three CSV files: <li>Users CSV File (ex. Users.csv)</li><li>Groups CSV File (ex. Groups.csv)</li><li>Group Members CSV File (ex. Members.csv)</li> |
| Pre/Post Operation Processing with PowerShell | This connector supports the configuration of up to four (4) PowerShell Scripts to facilitate pre-or-post processing operations.<br/>Script Execution Opportunities:<li>Before Import (ex. PreImport.ps1)</li><li>After Import (ex. PostImport.ps1)</li><li>Before Export (ex. PreExport.ps1)</li><li>After Export (ex. PostExport.ps1)</li> |
| CSV File Encoding Supported | The connector supports all default (or installed) server encoding types: (ex. Unicode, UTF-8, UTF-7, ASCII, etc.) |
| CSV Field Data Types Supported | The connector supports the following attribute data types: <li>Binary – (as 64-base strings)</li><li>Boolean – (as True/False)</li><li>Integers</li><li>Strings / Multivalued Strings</li><li>Reference / Multvalued References</li> |
| CSV Field Delimitation | Support for commas (,) or any printable alphameric character to qualify the beginning and end of any string value. |
| String Qualification Support | Support for double-quotes (“) or any printable alphameric character to qualify the beginning and end of any string value. |
| Multivalued String & Reference Support | Support for multivalued strings and references fields |
| Supported Connector Operations | The connector supports the following operations: <li>Full Import</li><li>Export</li><li>Full Export</li> |
| Schema | <p>Schema discovery is dynamic, but requires manual configuration for completion.</p><p>Fields are dynamically identified based upon a specified delimiter (or known as a "Value Separator.") </p><p>Field datatypes are manually designated during configuration.<p> |

### Prerequisites
Before you use the connector, make sure you have the following on the synchronization server:

* Microsoft .NET 4.6.2 Framework or later
* CSV Files that contain the desired schema for the following identity types:
  * Users File (required)
  * Groups (optional)
  * Group Members (required if groups are used)
* (Optional) PowerShell scripts to manage pre-and-post processing for the following Operation Types events:
  * Pre-Import – This script is executed before an import operation is run.
  * Post-Import – This script is executed after an import operation is run.
  * Pre-Export – This script is executed before an export operation is run.
  * Post-Export – This script is executed after an export operation is run.

#### MIM Synchronization Service Account Permissions
>[!IMPORTANT]
> The MIM 2016 Synchronization service account is the security context that performs the file operations to CSV files and runs the pre/post-processing PowerShell scripts. This service account needs Read/Write permissions for all the CSV and PowerShell files that are configured. It also needs the appropriate [PowerShell ExecutePolicy permissions](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4) to run any scripts that are configured.  

## Create a new Connector

To use the Generic CSV Connector in MIM 2016, the MIM administrator needs to perform the following steps:
* Create a management agent (MA) using the ECMA2Host extension.
* Select the Generic CSV Connector as the connector type.
* Provide the file path and name of the CSV file to be imported or exported.
* Specify the file encoding, value separator, multi-value separator, and text qualifier for the CSV file.
* Choose whether to use the values in the first row as header fields or not.
* Select the object types and attributes to be imported or exported from the CSV file.
* Configure the partition, run profile, and mapping details for the MA.
* Provide the script paths and parameters for the PowerShell scripts, if any.
* Run the MA to perform the import, sync, or export operations.

To Create a Generic SQL connector, in **Synchronization Service** select **Management Agent** and **Create**. Select the **Generic CSV (Microsoft)** Connector.

![CreateConnector page 1](./media/microsoft-identity-manager-2016-connector-genericcsv/createconnector.png)

### Connectivity

The Connectivity page contains the file locations of the User, Groups, and Group Members CSV files. 

The following image is an example of the Connectivity page. 

![CreateConnector page 2](./media/microsoft-identity-manager-2016-connector-genericcsv/connectivity.png)

On this page is where you specify the locations of the CSV files:  
* **Users File**: The fully qualified path of the CSV file that contains the user records and their attribute values. This file is required.
* **Groups File**: The fully qualified path of the CSV file that contains the group records. This file is optional. 
* **Members File**: The fully qualified path of the CSV file that contains group member reference records. 

>[!IMPORTANT]
>The MIM Sync service account must have **read** and **write** permissions to all the designated CSV files. As mentioned previously, the group and member files are not necessary if only users are configured.

The Connectivity screen is the first when you create a new Generic SQL Connector. You first need to provide The following section information:

### Capabilities

This page describes the connector’s capabilities. Connector capabilities are fixed and cannot be changed, but they are explained here to provide information on how the connector operates. 

The following image is an example of the Capabilities page. 

![Capablities image](./media/microsoft-identity-manager-2016-connector-genericcsv/capablities.png)

The following section lists of the individual configurations and their meanings:
*	**Distinguished Name Style (LDAP)**: The GCSV Connector uses the LDAP (Lightweight Directory Access Protocol) syntax to construct the DN (distinguished name) to uniquely identify each User or Group object in its connector space. All DN values are expressed in the following format: **CN=[ANCHOR_VALUE],Object=[User|Group],O=CSV**

*	**Object Conformation (Normal)**: Object conformation determines how the connector presents objects to the export script. When an attribute changes, the export process needs to decide which values to include for a multivalued attribute. In this case, the normal conformation means that the connector follows standard behavior:
    * When an attribute changes, it includes the full set of values for that attribute.
    * All existing values are replaced with the new ones.
  
* **Export Type (MultivaluedReferenceUpdateAdd)**: The export type specifies how objects are formatted and sent to the target system during synchronization.
*MultivaluedReferenceAttributeUpdate is an export type designed to work with Entra ID. It only sends the attributes that have changed. For non-reference attributes, it uses AttributeReplace and for reference attributes, it uses AttributeUpdate. 
* **Normalization (None)**: Normalizations refer to standardizing data to a consistent format. None means that no specific normalization rules are applied. Data remains as-is without any additional transformations by the connector.


### Schema 1 (File Configurations)

The GCSV Connector utilizes three kinds of separators (or delimiters) to delimit and parse CSV fields and their values. 

This page contains the character value settings for these separators and the encoding type that was used to create the file as CSV. 

The following image is an image of the Schema 1 (File Configurations) page.

![Schema1 image](./media/microsoft-identity-manager-2016-connector-genericcsv/schema1.png)

The following section is a list of the individual configurations:
* **Use headers for schema discovery**: When this option is selected, it instructs the connector to ignore the first record of each CSV file as a data record and use it as a header record (that is, that has the names of each field.) If this option isn't selected, the connector gives a generic name to each field (for example, Attribute1, Attribute2, etc.)  and use the first row as a data record.  

* **Values separator**: This character separates the fields (that is, values) of the CSV records. The comma (,) is the default, but any alphanumeric character that can be printed is allowed.  
* **Multivalue separator**: This type of separator is used to delimit the individual values of a multi–valued string (for example, proxy addresses) or reference attributes (for example, subordinates.) The default is a semi-colon (;) but any printable alphanumeric character is acceptable.
* **Text qualifier**: When a string value contains characters that would otherwise be interpreted as delimiters (for example, such as commas), it requires that the value be qualified so that the CSV parser can correctly interpret the string as a single field. The double quotes (") are the default, but any alphanumeric character that can be printed is allowed.

>**Note**
>Although the schemas of CSV files may not contain any multivalued fields or may not contain any values that require string qualification, the designation of a unique printable character for each separator type is required.

* **File encoding**: This setting indicates the encoding used on the CSV files added in the Connectivity tab. Ensure that it matches the encoding of your CSV files.

>**Note**
> If you are not sure about the encoding type of your CSV files, you should try to use the default Unicode encoding type. Unicode is a common standard that supports many characters and symbols, making it a good option for encoding text data across most languages or character set is used.


### Schema 2 (Anchor Configurations)

The anchor value is a unique identifier for a record in a CSV file. It differentiates one record from the others. The GCSV Connector also uses this value to create the distinguished name (DN) that identifies the related connector space object. 

On this page, the anchor attribute settings are set up for each of the CSV files that are listed on the Connectivity page. 

The following image is an example of the Schema 2 (Anchor Configurations) page.

![Schema2 image](./media/microsoft-identity-manager-2016-connector-genericcsv/schema2.png)

The following section is a list of the individual configurations on this page:
* **User**
  * **User Anchor**: The field in the Users file that serves as the anchor value for the user record. The first header column in the users file is the default choice.
  * **User Anchor attribute type**: This is the attribute type of the anchor selected. 
* **Group**
  * **Group Anchor**: The field in the Groups file that serves as the anchor value for the group record. The first header column in the users file is the default choice.
  * **Group Anchor attribute type**: This is the attribute type of the anchor selected.
* **Member**
  * **Parent Group ID**: The field in the Group Members file that has the same (anchor) value as the parent group in the Groups CSV file. The first column in the Members file is used by default.
  * **Member ID**: The field in the Group Members file that has the same (anchor) value as in the  Users or Groups CSV file. The second column found on the member file is selected by default.
* **Member Object Type**: The field that contains either a User or Group string value, to indicate object type of the member. This field is only required if the Member file contains more than two fields. The Object Type field must only contain either a **User** or a **Group** (string) value. If this field is missing, the connector will assume that record refers to a User object. The third column found on the member file is selected by default. 

>[!IMPORTANT]
>The names of the attributes designated to be used as anchors must be unique across all object type schemas. This includes the anchors specified in the Group Members file.

### Schema 3 (User File Attribute Configurations)
This page is for specifying and explaining the data type of each of the fields that are identified in the schema of the Users CSV file and whether they can have more than one value. 

The following image is an example of the Schema 3 (User File Attribute Configurations) page.

![Schema3 image](./media/microsoft-identity-manager-2016-connector-genericcsv/schema3.png)

The following section lists considerations when making attribute data type assignments.

#### Supported Data Types
The Generic CSV connector supports the use of The following section data types: 

* **Boolean**: a value that can be either true or false.
* **Binary**: a value that is stored as a sequence of bytes, typically used to store data such as images or other files.
* **Integer**: a value that is a whole number, without any decimal places.
* **String**: a value that is a sequence of characters, typically used to store text data.
* **Reference**: a value that is a reference to another user object. To specify a reference value in a CSV file, populate its field with the anchor value of the referred user object.

#### Supported Multiple-Value Data Types
The connector supports use of multivalued attributes for only the following data types:
* String
* Reference

> **Note**
> If the schema of both the User and Group objects both have an (non-anchor) attribute by the same name, differing datatypes may not be assigned between them. They both must share the same data type.

### Schema 4 (Group File Attribute Configuration)
This page is for specifying and explaining the data type of each of the fields that are identified in the schema of the Groups CSV file and whether they can have more than one value. 

The following image is an example of the Schema 4 (Group File Attribute Configuration) page. 

![Schema4a image](./media/microsoft-identity-manager-2016-connector-genericcsv/schema4a.png)

When providing configurations on this page, please refer to the considerations offered in the Schema 3 (User File Attribute Configurations) 

After the running an initial full import operation, the connector space will look similar to the image the following image: 

![Schema4a image](./media/microsoft-identity-manager-2016-connector-genericcsv/schema4b.png)

### Global Parameters (PowerShell Scripts Configuration)

This page allows for the configuration of PowerShell scripts that will run before and/or after import and/or export operations. This provides an opportunity to perform a wide variety of pre-and-post processing actions on your identity user and group records.

The following image is an example of the Global Parameters page. 

![GlobalParams image](./media/microsoft-identity-manager-2016-connector-genericcsv/globalparams.png)

The following section lists the individual configuration settings on this page:

* **Script Timeout (Minutes)**: the number of minutes that a script may run before it is automatically aborted. The default value for this setting is one hundred minutes (100) and requires a value greater than zero (0). 
* **Pre-import script file**: the fully qualified path to the PowerShell script that should run before an import. This setting is optional and does not require a value.
* **Post-import script file**: the fully qualified path to the PowerShell script that should run after an import. This setting is optional and does not require a value.
* **Pre-export script file**: the fully qualified path to the PowerShell script that should run before an export. This setting is optional and does not require a value.
* **Post-export script file**: the fully qualified path to the PowerShell script that should run after an export. This setting is optional and does not require a value.

#### PowerShell Script Execution and Input Parameters

The GCSV connector executes each of the configured PowerShell scripts in its own session and does not support the passing of parameters between stages isn't supported. 

The connector passes one input parameter into each script named OperationType. The value of this parameter varies depending on the Run Profile operation that is performed, and it can be one of three values: 
* Full – this value is provided during Full Import or Full Export operations.
* Delta – this value is provided during Export operations.

This parameter value can be used within the logic of the PowerShell scripts to determine the appropriate pre/post processing operation or action to take. 

>[!IMPORTANT]
> Additionally, the dynamic creation of CSV files before import or export operations is not supported. The all the CSV files must be present before any for Run Profiles will execute.
 
### Provisioning Hierarchy

Because CSV files do not store information in a hierarchical structure; the Generic CSV connector does not support any hierarchical provisioning configurations. 

The following image is an example of the Provisioning Hierarchy page.

![Provisioning image](./media/microsoft-identity-manager-2016-connector-genericcsv/provisioning.png)

### Partitions and Hierarchies

The Generic CSV Connector will build a distinct distinguished name (DN) for every user and group record in its connector space, following this LDAP format: 
CN=[ANCHOR_VALUE],Object=[User/Group],O=CSV

The following image is an example of the Partitions and Hierarchies page.

![Partitions image](./media/microsoft-identity-manager-2016-connector-genericcsv/partitions.png)

### Object Types

The Generic CSV connector requires that at least the User object type be specified. The choice of the Group object type is optional. 

The following image is an example of the Object Types page.

![ObjectTypes image](./media/microsoft-identity-manager-2016-connector-genericcsv/objecttypes.png)

### Attributes

This page displays a normalized list of all attributes across all selected object type schemas. 

The following image is an example of the Attributes page.

![Attributes image](./media/microsoft-identity-manager-2016-connector-genericcsv/attributes.png)

>[!NOTE]
> The Member attribute will only exist if Groups are selected and will contain the references to objects maintained in the group members CSV files.

### Anchors

The Generic CSV Connector does not support the use of complex anchors nor anchor attribute configurations that differ from their corresponding CSV file’s anchor ID fields. 

To change anchor designations displayed on this page, return to Schema 2 (Anchor Configurations). 

The following image is an example of the Anchors page.
 
![Anchors image](./media/microsoft-identity-manager-2016-connector-genericcsv/anchors.png)


## CSV Field Formatting Examples

The following sections list examples of how to format different datatypes in CSV files. All the examples The following section  assume the use of the connector’s default field delimiter settings:
* Value separate: Comma (,)
* Multivalue separator: Semi-Colon (;)
* Text qualifier: Double quotes (")

### Example: Text Qualification
If a string value contains characters that would otherwise be interpreted as delimiters (i.e., such as commas), it requires that the value be qualified so that the CSV parser can correctly interpret the string as a single field. 

The following section CSV example show how the DisplayName field has values that are formatted as qualified text:

```CSV
EmployeeID,DisplayName
E001,"Smith, John"
E002,"Doe, Jane"
E003,"Perez, Juan"
```

### Example: Delimiting Multivalued Strings
To provide multiple string values within one string field, simply delimit the values with the Multivalue separator. The following section CSV example shows how the ProxyAddress field with  multiple values:

```CSV
EmployeeID,DisplayName,ProxyAddresses
E001,"Smith, John",SMTP:john.smith@contoso.com;smtp:js001@contoso.com
E002,"Doe, Jane",SMTP:jane.doe@contoso.com;smtp:jd002@contoso.com
```

> **Note**
> Multivalued String also support the use of string qualified values.

### Example: Reference Fields
To specify a reference value in a CSV file, populate its field with the anchor value of the referred user object. In The following section CSV example, the Manager field contains the anchor value of the user record to which it refers: 

```CSV
EmployeeID,DisplayName,Manager
E001,"Smith, John",
E002,"Doe, Jane",E001
E003,"Doe, Jane", 
E004,"Perez, Juan",
```
### Example: Binary Fields

To express binary values in CSV files, they must be converted to base 64 strings that use the same encoding type as the CSV file. 
The following section PowerShell function demonstrates how to encode a string value into its base 64 encoded string in Unicode:

~~~PowerShell
function ConvertTo-Base64([string]$text) 
{
    $bytes = [System.Text.Encoding]::Unicode.GetBytes($text)
    $encodedText = [System.Convert]::ToBase64String($bytes)
    return $encodedText
}
~~~

Here is the equivalent function in C# that accepts an input parameter called text and returns a base64 encoded string in Unicode

~~~C#
public static string ConvertToBase64(string text)
{
    byte[] bytes = System.Text.Encoding.UTF8.GetBytes(text);
    string encodedText = System.Convert.ToBase64String(bytes);
    return encodedText;
}
~~~

### Example: Boolean Fields

CSV Files that contain Boolean fields should use either the True or False text to indicate their value. The following section  is an

```CSV
EmployeeID,DisplayName,IsActive
E001,"Smith, John",true
E002,"Doe, Jane",true
E003,Perez, Juan",false
```

## Troubleshooting
* For information on how to enable logging to troubleshoot the connector, see the How to Enable ETW Tracing for Connectors.
