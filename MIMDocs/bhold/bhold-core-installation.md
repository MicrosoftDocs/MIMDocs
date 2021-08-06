---
# required metadata

title: BHOLD core Installation | Microsoft Docs
description: BHOLD suite installation core document
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid:


---
# BHOLD Core installation

The BHOLD Core module provides the key features of BHOLD Suite within your environment. The BHOLD Core module must be installed and configured on a server in your local area network before you can install other BHOLD Suite modules.

## BHOLD Core installation requirements

The BHOLD Core module forms the foundation of Microsoft BHOLD Suite. You must install the BHOLD Core module before installing other Microsoft BHOLD Suite modules.

### BHOLD Core hardware requirements

The BHOLD Core module forms the foundation of Microsoft BHOLD Suite. You must install the BHOLD Core module before installing other Microsoft BHOLD Suite modules.

|          |        |          |
|----------|--------|----------|
|**Component** |**Minimum**	| **Recommended** |
|Processor | 64-bit processor | Multicore 64-bit processor |
| Memory |3 GB | 6 GB or more |
|Storage| 30 GB available |Depends on deployment size |
|Network adapter| 100Mbps connection to SQL and Forefront Identity Manager (FIM) server | 1Gbps connection to SQL and FIM server|

These recommendations are based on typical implementations and do not take into consideration other applications running on the server. You might need to use higher-performing components depending on your particular environment.

### BHOLD Core software requirements

The BHOLD Core module must be installed on a computer that meets the following requirements:

- The server must be running Windows Server 2012 (64-bit), Windows Server 2016 
- The server must be a member of an Active Directory Domain Services (AD DS) domain. In test environments, the server may be an AD DS domain controller.
- BHOLD Core must be installed by a user logged on with an account in the same domain as the server and which belongs to the Domain Admins group in the domain and to the Administrators group on the server.
- Microsoft Internet Information Services (IIS) with ASP.NET must be installed on the server. IIS must be configured with Windows Authentication enabled. If BHOLD Core is being installed on Windows Server 2012/2016, IIS 6 Management Compatibility must be installed. If BHOLD Core is being installed on Windows Server 2012, IIS 6 Scripting tools must be installed
- The .NET 3.5 framework must be installed.
  - Because BHOLD Core requires .NET 3.5, it cannot be installed on Server Core.
- Silverlight 4 is required by other BHOLD modules, and so it is recommended that you install it prior to installing BHOLD Core.
- Microsoft SQL Server 2014 or Microsoft SQL Server 2016 must be installed either on the BHOLD Core server or on another server on the LAN. 

Windows Client computers must be running Microsoft Internet Explorer version 6 or later and Microsoft Silverlight version 4 or later.

## Before you begin

The BHOLD Core module requires a user account which is used to authenticate and authorize the BHOLD Core service to the server and other network entities. This section explains how to create and configure that user account and provides a preinstallation worksheet that will help you gather the information you need to complete BHOLD Core Setup.

>{!IMPORTANT}
When you install BHOLD Core, you can use an existing SQL Server database as the BHOLD Core database, or you can allow the BHOLD Core Setup wizard to create the BHOLD Core database for you. If you choose to use an existing database, you must ensure that no other users can access the database while you are installing BHOLD Core. Before installing BHOLD Core, verify that the access controls of the SQL Server database do not permit access by anyone other than the user who is installing BHOLD Core.

## Required user and group

The BHOLD Core module must be able to log on to the domain with a user account that is dedicated to that purpose and which is a member of two specific security groups, including one that is created specifically for the BHOLD Core module. Membership in the Domain Admins group is required to perform this procedure.
**To create and configure the BHOLD Core user and security group**

1.  On a domain controller, click **Start**, point to **Administrative Tools**,  and then click **Active Directory Users and Computers**.

2.  In the console tree, expand the domain where the account is to be created, right-click **Users**, point to **New**, and then click **Group**.

3.  In the **New Object – Group** dialog box, in **Group name**, type the name of the group (BHOLD default: BHOLDApplicationGroup), and then click **OK**.

4.  Right-click **Users**, point to **New**, and then click **User**.

5.  In **Full name**, type a name that will help you identify the account, for example, BHOLD Core Service Account.

6.  In **User logon name**, type the user name of the BHOLD Core service account (BHOLD default: b1user), and then click **Next**.

7.  In **Password** and **Confirm password**, type the password for the service     account.

8.  Clear **User must change password at next logon**, select **User cannot change password** and **Password never expires**, click **Next**, and then click **Finish**.

9.  In the console results pane, right-click the user account, and then click **Add to a group**.

10. In the **Select Groups** dialog box, type the display name of the group that you created earlier, type a semicolon (;), and then type IIS_IUSRS.

11. Click **Check Names**, and then click **OK**.  

The following procedure must be performed on the computer where the BHOLD Core module is to be installed. You must be logged in as a member of the Domain Admins group to perform this procedure.

## BHOLD Core installation worksheet

Before you begin to install the BHOLD Core module, you need to be prepared to provide the information that the BHOLD Core Setup wizard requires to complete the installation. The following worksheet will help you record that information so you will be ready to supply it when it is needed. Each section corresponds to a page in the BHOLD Core setup wizard.

### Account settings

| **Item**                                    | **Description**                                                                                                                                                                                                                                                                                             | **Value**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** | When selected, specifies that Active Directory Domain Services security will control access to BHOLD Core.                                                                                                                                                                                                  | Select the check box. **Important:** The installation will fail if this check box is not selected.                                                                 |
| **Domain**                                  | Specifies the domain containing the BHOLD server, service account, and application group. **Important:** Specify the domain name by using the NetBIOS (short) name, not the fully qualified domain name (FQDN). For example, if the FQDN of the domain is fabrikam.com, specify the domain name as CONTOSO. | Write the domain name here:                                                                                                                                        |
| **Application group**                       | Specifies the name of the security group that you created previously in [Required user and group](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Write the group name here:                                                                                                                                         |
| **Service user**                            | Specifies the logon name of the service user account that you created previously in [Required user and group](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Write the user account name here:                                                                                                                                  |
| **Password**                                | Specifies the password of the BHOLD Core service user account.                                                                                                                                                                                                                                              | Write the password here: **Important:** Be sure to keep this password in a hidden, secure location.                                                                |
| **Website IP/Port**                         | Specifies the IP address and port number of the website to be created on the intranet server. Change the default value (\*) only if you will not use the same IP address as the default website. Change the port number to an available port only if the default port (5151) is already in use.             | If a nondefault IP address is used by the default website, write it here:  If the default port number is already in use, write the BHOLD website port number here: |

### Database settings

| **Item**                                       | **Description**                                                                                                                                                                                                                                                           | **Value**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use integrated Security**                    | Specifies that Windows Authentication is used to access the database.                                                                                                                                                                                                     | Select the check box if Windows Authentication is used to connect to the SQL Server. Clear the check box if SQL Server Authentication is used. The database must have been created before running BHOLD Core Setup If SQL Server Authentication is used. **Note:** If Windows Authentication is used, you must be logged on with an account that has the sysadmin server role on the database server. |
| **Database User**  and   **Database Password** | Specifies the user name and password of a user with the sysadmin server role on the database server. These values are supplied only when SQL Server Authentication is used.                                                                                               | Write the SQL Server user name here:  Write the SQL Server user password here: **Note:** Be sure to keep this password in a hidden, secure location.                                                                                                                                                                                                                                                  |
| **Database Server**  and   **Database Name**   | Specifies the NetBIOS name of the database server and the name of the database (default: b1) that BHOLD Core Setup will create. If you are not using the default database server instance, specify the database server instance in the form *\<server\>*\\*\<instance\>*. | Write the server (or server and instance) name here:  Write the database name here:                                                                                                                                                                                                                                                                                                                   |
| **Make restrictions for the database user**    | Obsolete.                                                                                                                                                                                                                                                                 | Do not change the default value                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## BHOLD Core Setup

To install the BHOLD Core module, log on as a member of the Domain Admins group, download the following file and run it as administrator on the server that you intend to install the BHOLD Core module on: 

- BholdCore *\<Version\>*\_Release.msi

Replace *\<Version\>* with the version number of the BHOLD Core release that you are installing.

To run the program file as an administrator, right-click the file and then click **Run as administrator**.

### Postinstallation settings

After BHOLD Core Setup completes, you must configure the Windows Firewall and change advanced settings in the BHOLD Core application pool in Internet Information Services to complete the BHOLD Core configuration. If necessary, you should also change BHOLD system attributes to meet your requirements.

#### Configuring Windows Firewall

If users will access BHOLD by using web browsers on remote computers, you must configure Windows Firewall on the BHOLD Core server to allow incoming connections to the website port that you specified when you installed BHOLD Core.

To perform this procedure, you must be a member of the Administrators group on the local computer.

#### To permit incoming connections to the BHOLD website

1.  Click **Start**, point to **Administrative Tools**, right-click **Windows Firewall with Advanced Settings**, and then click **Run as administrator**.

2.  In the left pane, click **Inbound Rules**, and then, in the right pane, click **New Rule**.

3.  In the New Inbound Rule wizard, click **Port**, and then click **Next**.

4.  Ensure that **TCP** is selected, in **Specific local ports**, type the default BHOLD Core port number (5151) or the port number that you specified when you installed BHOLD Core, and then click **Next**.

5.  Ensure that **Allow the connection** is selected, and then click **Next**.

6.  On the **Profile** page, clear check boxes for locations from which you do not want access to the BHOLD website, and then click **Next**.

7.  On the **Name** page, type a name for the rule (such as Permit incoming connections to BHOLD Core), and then click **Finish**.  

#### Enabling 32-bit applications for the BHOLD Core application pool

To allow IIS to work properly with the BHOLD Core module, you must configure the BHOLD Core application pool to support 32-bit applications. You must be logged in with the account used to install the BHOLD Core module to perform this procedure.

**To enable 32-bit application support for the BHOLD Core application pool**

1.  To open Internet Information Services Manager, click **Start**, point to **Administrator Tools**, and then click **Internet Information Services (IIS) Manager**.

2.  In the console tree, expand the server name, and then click **Application Pools**.

3.  In the **Application Pools** list, right-click **CoreAppPool**, and then  click **Advanced Settings**.

4.  In the **Advanced Settings** dialog box, in the **Enable 32-Bit Applications** list, select **True**, and then click **OK**.

#### Establishing the service principal name for the BHOLD website

If the network name that is used to contact the BHOLD website is not the same as the server host name, you must establish a service principal name (SPN) for HTTP. For example, if you use a CNAME resource record in DNS to specify an alias for the server, or if you use network load balancing, you must register these additional network addresses in Active Directory. If you fail to do so, Internet Explorer cannot use the Kerberos protocol when contacting the BHOLD website.

> [!IMPORTANT]
> If the BHOLD Core module is installed on the same computer as the FIM Portal, you must create DNS resource records (CNAME or A) with different host names for the servers running BHOLD Core and the server running the FIM Portal. Only one SPN can be established for a particular service-type/server-alias pair, and so BHOLD Core and the FIM Portal require separate SPNs because they typically run under different accounts. The setspn command reports an error if an SPN has already been established under another account.

Membership in **Domain Admins**, or equivalent, is the minimum required to complete this procedure.

#### To establish the SPN of the BHOLD website

1.  On the Active Directory Domain Services domain controller, click **Start**, click **All programs**, click **Accessories**, right-click **Command Prompt**, and then click **Run as administrator**.

2.  At the command prompt, type the following command, and then press ENTER:
    setspn –S HTTP/ *\<networkalias\> \<domain\>* \\ *\<accountname\>*
    where:

    -   *\<networkalias\>* is the address that clients use to contact the BHOLD website

    -   *\<domain\>*\\*\<accountname\>* is the domain and user name of the BHOLD Core service account that you created when you installed BHOLD Core.

3.  Repeat the previous step for all other names that clients use to contact the BHOLD website, for example, CNAME aliases, names that include a fully qualified domain name, or names that include a NetBIOS (short) domain name.

#### Setting BHOLD system attributes

In order to validate that the installation of the BHOLD Core module was successful, open the BHOLD Core portal and view the system attributes. In addition, to ensure that the BHOLD Core module functions properly in your environment, you can modify the following BHOLD system attributes, as appropriate:

| **Attribute**                | **Description**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Set to Y if the BHOLD website is running on a clustered web service to ensure that recently displayed items work properly. Set to N if the BHOLD website is running on a standalone IIS server.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Set to Y to ensure that organizational units (orgunits) can be moved only to orgunits with the same organizational type as the parent orgunit. For example, this prevents a project orgunit from being moved into a department orgunit. Set to N to allow an orgunit to be placed in an orgunit of a different type. |
| **Days between ABA run**     | Set to a two-digit integer to specify the interval (in days) between two attribute-based authorization (ABA) runs. For example, to specify that ABA runs will be separated by two days, type 02.                                                                                                                     |
| **Start hour of ABA run**    | Set to a two-digit integer to specify the hour of the day when an attribute-based authorization run will occur. For example, to specify that the ABA run will take place at 11:00 p.m. (23:00), type 23.                                                                                                             |
| **System Cardinality**       | Set to N if you do not want a system cardinality check in BHOLD. The default value is Y.                                                                                                                                                                                                                             |
| **Logging**                  | Set to N if you do not want changes to be logged. The default value is Y.                                                                                                                                                                                                                                            |
| **SystemQueue Processing**   | Set to N if you do not want system queue processing. Do not change this value unless directed to do so by product support.                                                                                                                                                                                           |

You must be logged in as a member of the Domain Admins group to perform this procedure.

**To set a BHOLD system attribute**

1.  Click **Start**, click **All Programs**, and then click **Internet Explorer**.

2.  In the address box, type , where *\<server\>* is the name of the BHOLD website server and *\<port\>* is the port number bound to the website.

3.  Click **Home**, click **Values**, and then click **Modify**.

4.  Locate the name of the attribute you want to change, type the new value in the box next to the attribute name, and then click **OK**.

## Next steps

After you have installed BHOLD Core and validated that the installation was successful, you can install additional modules. At this point, the BHOLD database will be essentially empty, containing only one user account, the root account, and one organizational unit (orgunit), the root orgunit. To add more users to the BHOLD database, you can install the Access Management Connector module to import user data from the FIM Synchronization Service. For more information about using the Access Management Connector module, see [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

For more information about using the BHOLD Model Generator module, see:

- [Microsoft BHOLD Suite Concepts Guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
