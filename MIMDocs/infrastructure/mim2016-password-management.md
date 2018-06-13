---
# required metadata

title: Microsoft Identity Manager 2016 Password Management| Microsoft Docs
description:
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---


# Microsoft Identity Manager 2016 Password Management

Managing passwords for multiple user accounts is one of the complexities of
managing an enterprise environment with multiple data sources. Microsoft
Identity Manager 2016 (MIM) provides two password management
solutions:

-   Password synchronization – Utilizes the password change notification service
    (PCNS) to capture password changes from Active Directory and propagate them
    to other connected data sources.

-   User-based password change management – Utilizes the Windows Management
    Instrumentation (WMI) through Web-based Help Desk and self-service password
    reset applications.

By using password synchronization and user-based password change management, you
can:

-   Reduce the number of different passwords users have to remember.

-   Simultaneously set or change passwords in a user's multiple accounts to the
    same password.

-   Allow users to change their own passwords in Active Directory and push the
    password change out to other systems.

-   Eliminate the risk of building an additional password or credential store.

-   Synchronize passwords across multiple data sources by using Active Directory
    as the authoritative source.

-   Perform password management operations in real time, independent of MIM
    operations.

## Password extensions

Management agents for directory servers support password change and set
operations by default. For file-based, database, and extensible connectivity
management agents, which do not support password change and set operations by
default, you can create a .NET password extension dynamic-link library (DLL).
The .NET password extension DLL is called whenever a password change or set call
is invoked for any of these management agents. Password extension settings are
configured for these management agents in Synchronization Service Manager. For
more information about configuring password extensions, see the FIM Developer
Reference.

| Password management is supported by default in the management agents for: | By using a password extension, password management is also supported in the management agents for: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Attribute-value pair text files                                                                    |
| Active Directory Lightweight Directory Services (ADLDS)                   | Delimited text files                                                                               |
| IBM Directory Server                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Fixed-width text files                                                                             |
| Sun and Netscape directory servers                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | LDAP Data Interchange Format (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle Database                                                                                    |

## Password synchronization


Password synchronization works with the password change notification service
(PCNS) on an Active Directory domain, and allows password changes that originate
from Active Directory to be automatically propagated to other connected data
sources. MIM accomplishes this by running as a Remote Procedure Call (RPC)
server that listens for a password change notification from an Active Directory
domain controller. When the password change request is received and
authenticated, it is processed by MIM and propagated to the appropriate
management agents.

> [!IMPORTANT]
> Bi-directional password synchronization is not supported by MIM. Configuring bi-directional password synchronization can create a loop, which will consume server resources and have a potentially negative effect on both Active Directory and MIM.

The PCNS runs on each Active Directory domain controller. The systems that
receive the password notifications are known as targets. Your MIM server must be
configured as a PCNS target in Active Directory before password notifications
are sent. The PCNS configuration must define an inclusion group and, optionally,
an exclusion group. These groups are used to restrict the flow of sensitive
passwords from the domain. For example, to send passwords for all users, but not
send administrative passwords, you might choose to use Domain Users as the
inclusion group, and Domain Admins as the exclusion group. For more information
about configuring the password change notification service, see [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx)

The components involved in the password synchronization process are:

-   **Password change notification service (Pcnssvc.exe)**–The password change
    notification service runs on a domain controller and is responsible for
    receiving password change notifications from the local password filter,
    queuing them for the target server running MIM, and using RPC to deliver the
    notifications. The service encrypts the password and ensures that the
    password remains secure until it is successfully delivered to the target
    server running MIM.

-   **Service principal name (SPN)** – The SPN is a property on the account object
    in Active Directory that is used by the Kerberos protocol to mutually
    authenticate the PCNS and the target. The SPN ensures that the PCNS
    authenticates to the correct server running MIM, and that no other service
    can receive the password change notifications. The SPN is created and
    assigned by using the setspn.exe tool. For more information about
    configuring the SPN, see Using Password Synchronization.

-   **Password change notification filter (Pcnsflt.dll)** – The password filter is
    used to obtain plaintext passwords from Active Directory. This filter is
    loaded by the Local Security Authority (LSA) on each Windows Server domain controller participating in password distribution to a target server running MIM. Once the filter has been installed and the domain controller has been restarted, the filter begins to receive password change notifications for password changes that originate on that domain controller. The password notification filter runs simultaneously with other filters that are running on the domain controller.

-   **Password change notification service configuration utility (Pcnscfg.exe)** –
    The pcnscfg.exe utility is used to manage and maintain the password change
    notification service configuration parameters stored within Active
    Directory. These configuration parameters, such as defining the target
    servers, the password queue retry interval, and enabling or disabling a
    target server, are used when authenticating and sending password
    notifications to the target server running MIM.
    The service configuration is stored in Active Directory, so it is only
    necessary to update the configuration on one domain controller. Active
    Directory replicates the change to all other domain controllers.

-   **Remote Procedure Call (RPC) server on the server running MIM** – When password
    synchronization is enabled, the RPC server on the server running MIM is
    started, enabling it to receive notifications from the password change
    notification service. RPC dynamically selects a range of ports to use. If
    you require MIM to communicate with the Active Directory forest through a
    firewall, you must open a range of ports.

-   **Password extension DLL** – The password extension DLL provides a way to
    implement password set or change operations by means of a rules extension
    for any database, extensible connectivity, or file-based management agent.
    This is accomplished by creating an export-only, encrypted attribute named
    "export_password" that does not actually exist in the connected directory
    but can be accessed and set in provisioning rules extensions or can be used
    during export attribute flow. For more information about configuring
    password extensions, see the [FIM Developer Reference](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## Preparing for password synchronization

Before you set up password synchronization for your MIM and Active Directory
environment, verify the following:

-   MIM is installed according to installation instructions.

-   Management agents for the connected data sources to be managed for password
    synchronization are already created and the objects are being successfully
    joined and synchronized.

To set up password synchronization:

-   Extend the Active Directory schema to add the classes and attributes
    necessary for installing and running the password change notification
    service (PCNS).

-   Install the PCNS on each domain controller.

-   Configure the service principal name (SPN) in Active Directory for the MIM
    service account.

-   Configure the PCNS to communicate with the target MIM service.

-   Configure the management agents for the connected data sources to be managed
    for password synchronization.

-   Enable password synchronization on MIM.

For more information about setting up password synchronization, see Using
Password Synchronization.

## Password synchronization process

The process of synchronizing a password change request from an Active Directory
domain controller to other connected data sources is shown in the following
diagram:

1.  The user initiates the password change request by pressing Ctrl+Alt+Del. The
    password change request, including the new password, is sent to the nearest
    domain controller.

2.  The domain controller records the password change request and notifies the
    password change notification filter (Pcnsflt.dll).

3.  The password change notification filter passes the request to the password
    change notification service (PCNS).

4.  The PCNS verifies the password change request, then authenticates the
    service principal name (SPN) by using Kerberos, and forwards the password
    change request in encrypted RPC to the MIM target server.

5.  MIM validates the source domain controller, then uses the domain name to
    locate the management agent that services that domain, and uses the user
    account information in the password change request to locate the
    corresponding object in the connector space.

6.  By using the join table information, MIM determines the management agents
    that receive the password change, and pushes the password change out to
    them.

## Password synchronization security

The following password synchronization security concerns have been addressed:

-   Authentication from the password source – When the password change
    notification is received, Kerberos authentication is done by MIM as well as
    the source domain controller to ensure both the recipient and sender are
    valid. Upon receiving a password change notification, MIM ensures that the
    caller has an account in the Domain Controllers container of the domain it
    belongs to.

-   Failed password synchronization to a target data source due to an insecure
    connection – If the management agent has been configured to require a secure
    connection but one is not detected, the synchronization fails.
    Synchronization still occurs if the management agent has been configured to
    allow nonsecure connections. Allowing non-secure connections should be
    enabled only after examining and understanding the risks involved.

-   Secure storage of passwords – MIM only stores encrypted passwords
    temporarily. All passwords received by MIM during a password change
    notification operation are encrypted as soon as they enter the MIM process.
    The moment they are successfully sent out to the target connected data
    source, they are decrypted, and the memory storing the password is
    immediately cleared. If the operation fails to write to the target connected
    data source, the encrypted password is stored until all retry attempts have
    been attempted, and then is cleared from memory.

-   Secure password queues – Passwords stored in PCNS password queues are
    encrypted until they are delivered.

## Password synchronization error recovery scenarios

Ideally, whenever a user changes a password, the change is synchronized with no
errors. The following scenarios describe how MIM recovers from common
synchronization errors:

-   **Failed password notification from Active Directory to MIM** – This can occur
    if the network is down, or if the server running MIM is unavailable. The
    password change notification remains queued locally on the domain controller
    by the PCNS. The PCNS reattempts the notification according to its retry
    interval configuration.

-   **Failed password synchronization to a target data source** – This can also
    occur if the network is down, or if the target data source is unavailable.
    The password change notification is queued and retried according to the
    management agent's configuration for retry attempt and retry interval. All
    passwords are encrypted while they are stored for retry, and deleted when
    the operation succeeds or the retry limits are hit.

-   **Activating a warm standby server running MIM after a failure** – In the case
    of the primary server running MIM failing, you can configure a warm standby server for password synchronization, and activate it with no loss of password changes. For more information, see [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx)

Some failures are serious enough that no amount of retries is likely to result
in a successful operation. In these cases, an error event is logged and the
process is stopped. The following events are not retried:

| Event | Severity    | Description                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Information | A password synchronization set operation was not performed because the timestamp was out of date.                                                                      |
| 6921  | Error       | The password synchronization set operation was not processed because password management is not enabled on the target management agent.                                |
| 6922  | Error       | The password synchronization set operation was not processed because password management is not configured on the target management agent.                             |
| 6923  | Warning     | The password synchronization set operation was not processed because the target connector space object could not be found in the connected directory.                  |
| 6927  | Error       | The password synchronization set operation failed because the password does not satisfy the password policy of the target system.                                      |
| 6928  | Error       | The password synchronization set operation failed because the password extension for the target management agent is not configured to support password set operations. |

## User-based password change management

MIM provides two web applications that use Windows Management Instrumentation
(WMI) for resetting passwords. As with password synchronization, you activate
password management when you configure the management agent in Management Agent
Designer. For information about password management and WMI, see the MIM
Developer Reference.

MIM creates two security groups during installation that specifically support
password management operations:

-   FIMSyncBrowse—Members of this group have permission to gather information
    about a user's accounts when doing search operations with WMI queries.

-   FIMSyncPasswordSet—Members of this group have permission to perform account
    search, password set, and password change operations using the password
    management interfaces with WMI.
