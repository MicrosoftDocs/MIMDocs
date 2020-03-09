---
# required metadata

title: BHOLD model generator installation | Microsoft Docs
description: BHOLD model allows you to structure data from various sources 
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid:


---

# BHOLD Model Generator Installation

By using the BHOLD Model Generator module, you can structure data from authoritative sources that contains user and organizational information along with access control lists (ACLs) into a model that can be used in administering BHOLD.

## BHOLD Model Generator installation requirements 

Before installing the BHOLD Model Generator module, you must install the
following:

1. BHOLD Core module on the server on which you plan to install the BHOLD Model Generator module. For information about installing the BHOLD Core module, see [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. The Microsoft OLE DB Provider for Microsoft Jet must be installed. For more information see [this article](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Do not install BHOLD Model Generator in your production network. BHOLD Model Generator is intended to be used offline in a staging environment to create a normalized role model that you can import into your enterprise role model. Running BHOLD Model Generator in your production network can result in loss of your existing role model.

## Before you begin

Before installing the BHOLD Model Generator module, you need to be prepared to
provide the information that the BHOLD Model Generator Setup wizard requires to
complete the installation. The following worksheet will help you record that
information so you will be ready to supply it when it is needed. You will also
need to make sure

Microsoft Access Database Engine 2010 Redistributable

 

*From \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Account settings**

| **Item**                                    | **Description**                                                                                                                                                                                                           | **Value**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** | When selected, specifies that Active Directory Domain Services security will control access to BHOLD Core.                                                                                                                | Select the check box. **Important:** The installation will fail if this check box is not selected.                                                                                                                                                                                                                   |
| **Domain**                                  | Specifies the domain containing the service account that you created when installing BHOLD Core. For more information, see [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | The domain name is supplied automatically by the wizard. Change the name only if it is incorrect. **Important:** Specify the domain name by using the NetBIOS (short) name, not the fully qualified domain name (FQDN). For example, if the FQDN of the domain is fabrikam.com, specify the domain name as FABRIKAM. |
| **User**                                    | Specifies the logon name of the BHOLD Core service user account.                                                                                                                                                          | Write the user account name here:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifies the password of the service user account.                                                                                                                                                                       | Write the password here: **Important:** Be sure to keep this password in a hidden, secure location.                                                                                                                                                                                                                  |

**Backup database settings**

| Item                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use integrated Security**                 | Specifies that Windows Authentication is used to access the database.                                                                                                                                                                                                                                                                                                                                                        | Select the check box if Windows Authentication is used to connect to the SQL Server. Clear the check box if SQL Server Authentication is used. The database must have been created before running BHOLD Core Setup If SQL Server Authentication is used. **Note:** If Windows Authentication is used, you must be logged on with an account that has the sysadmin server role on the database server. **Important:** Use SQL Server Authentication only in test environments. Microsoft strongly recommends using Windows Authentication in production deployments. |
| **Database User** and **Database Password** | Specifies the user name and password of a user with the sysadmin server role on the database server. These values are supplied only when SQL Server Authentication is used.                                                                                                                                                                                                                                                  | Write the SQL Server user name here:  Write the SQL Server user password here: </br></br> **Important:** Be sure to keep this password in a hidden, secure location.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Database Server** and **Database Name**   | Specifies the NetBIOS name of the database server and the name of the backup database that BHOLD Model Generator Setup will create. If you are not using the default database server instance, specify the database server instance in the form *\<server\>*\\*\<instance\>*.  Microsoft recommends that you name the backup database using the name of the BHOLD Core database followed by \_BACKUP, for example B1_BACKUP. | Write the server (or server and instance) name here: </br> Write the database name here:

## BHOLD Model Generator setup

To install the BHOLD Model Generator module, log on as a member of the Domain Admins group, download the following file and run it as administrator on the server that you intend to install the BHOLD Core module on:

- BholdModelGenerator *\<Version\>*\_Release.msi

Replace *\<Version\>* with the version number of the BHOLD Model Generator release that you are installing.

To run the program file as an administrator, right-click the file and then click **Run as administrator**.

## Next steps

- For information on how to create input files [Microsoft BHOLD Suite Technical Reference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [BHOLD installation guide](bhold-installation-guide.md)
- [BHOLD developer reference](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD version history](../reference/version-bhold-history.md)
