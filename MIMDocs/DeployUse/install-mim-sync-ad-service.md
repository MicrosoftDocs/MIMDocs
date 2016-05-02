---
# required metadata

title: Install MIM 2016&#58; Synchronize Active Directory and MIM Service | Microsoft Identity Manager
description: Use management agents and the MIM Sync Service to sync your Active Directory and MIM databases.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Install MIM 2016: Synchronize Active Directory and MIM Service

>[!div class="step-by-step"]
[« MIM Service and Portal](install-mim-service-portal.md)

> [!NOTE]
> This walkthrough uses sample names and values from a company called Contoso. Replace these with your own. For example:
> - Domain controller name - **mimservername**
> - Domain name - **contoso**
> - Password - **Pass@word1**

By default, MIM Synchronization Service (Sync) does not have any connectors configured.  A typical first step is to use MIM Sync to populate the MIM Service database with existing Active Directory accounts. For this, you will use the MIM Sync Service application.

## Create the MIM management agent
The MIM management agent (MA) is a connector for MIM Sync to the MIM Service. To create this connector, you use the Create Management Agent wizard.

When you configure a MIM management agent, you need to specify a user account. This document uses **MIMMA** as the name for this account.

> [!NOTE]
> The account you use for your MIM management agent must be the same account as the one you have specified during the installation of MIM Service.

###To create the MIM MA

1.  Open the Synchronization Service Manager.

2.  To open the Create Management Agent wizard, on the **Actions** menu, click **Create**.

3.  On the **Create Management Agent** page, provide the following settings, and then click **Next**.

    -   Management agent for: MIM Service management agent

    -   Name: MIMMA

4.  On the **Connect to Database** page, provide the following settings, and then click **Next**

    -   Server: localhost

    -   Database: MIMService

    -   MIM Service base address: http://localhost:5725

    -   Authentication mode: Windows integrated authentication

    -   User name: mimma

    -   Password: Pass@word

    -   Domain: contoso

5.  On the **Selected Object Types** page, verify that the object types that are listed below are selected, and then click **Next**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   Group

6.  On the **Selected Attributes** page, verify that all listed attributes are selected, and then click **Next**.

7.  On the **Configure Connector Filter** page, click **Next**.

8.  On the **Configure Object Type Mappings** page, add the following mapping, and then click **Next**

    - Select **Person** in the **Data Source Object Type** list.
    - Click **Add Mapping** to open the Mapping dialog box.
    - Select **person** in the **Metaverse object type** list.
    - Click **OK** to close the Mapping dialog box.

9.  On the **Configure Attribute Flow** page, apply the following attribute flow mappings, and then click **Next**

    | **Flow Direction** | **Data Source Attribute** | **Metaverse Attribute** |
    |-|-|-|
    |Import|Import|accountName|
    |Import|Import|company|
    |Import|Import|displayName|
    |Import|Import|employeeID|
    |Import|Import|employeeType|
    |Import|Import|firstName|
    |Import|Import|lastName|
    |Import|Import|Manager|
    |Import|Import|objectSid|
    |Export|Export|accountName|
    |Export|Export|company|
    |Export|Export|displayName|
    |Export|Export|domain|
    |Export|Export|employeeID|
    |Export|Export|employeeType|
    |Export|Export|firstName|
    |Export|Export|lastName|
    |Export|Export|manager|
    |Export|Export|objectSid|

10.  Select **Person** as the Data source object type.

    -   Select **Person** as the Metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    -   Select **Group** as the data source type and as the metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the following table, complete these steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    | Flow Direction | Data Source Attribute | Metaverse Attribute |
    |-|-|-|
    | Export | AccountName | accountName |
    | Export | DisplayName | displayName |
    | Export | Domain | domain |
    | Export | Scope | scope |
    | Export | Type | type |
    | Export | Member | member |
    | Export | MembershipLocked | membershipLocked |
    | Export | MembershipAddWorkflow | membershipAddWorkflow |
    | Export | Manager | manager |

11.  On the **Configure Deprovisioning** page, click **Next**

12.  To create the management agent, on the **Configure Extensions** page, click **Finish**.

## Create the AD management agent
The Active Directory management agent is a connector for AD Domain Services. To create this connector, you use the Create Management Agent wizard.

1. To open the Create Management Agent wizard, on the **Actions** menu, click **Create**.

2. On the **Create Management Agent** page, provide the following settings, and then click **Next**:

    - Management agent for: Active Directory Domain Services
    - Name: ADMA

3. On the **Connect to Active Directory Forest** page, provide the following settings, and then click **Next**:

    - Forest name: contoso.local
    - User name: administrator
    - Password : &lt;the account’s password&gt;
    - Domain: contoso

4. On the **Configure Directory Partitions** page, provide the following settings, and then click **Next**:

    - In the **Select directory partitions** list, select **DC=CONTOSO, DC=local**.

    - To open the Select Containers dialog box, click **Containers**.

    - If you wish to change the container to only have MIM manage objects in a particular container, click the **DC=CONTOSO,DC=local** node, and then click the node for the container of interest.

    - To close the Select Containers dialog box, click **OK**.

5. On the **Configure Provisioning Hierarchy** page, click **Next**.

6. On the **Select Object Types** page, provide the following settings, and then click **Next**:

    - In the **Object types** list, select **user** and **group**.

7. On the **Select Attributes** page, provide the following settings, and then click **Next**:

    - Select **Show All**.

8. In the **Attributes** list, select the following attributes:

    -   company
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   groupType
    -   manager
    -   managedBy
    -   member
    -   objectSid
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   unicodePwd
    -   userAccountControl

9. On the **Configure Connector Filter** page, click **Next**.

10. On the **Configure Join and Projection Rules** page, click **Next**.

11. On the **Configure Attribute Flow** page, click **Next**.

12. On the **Configure Deprovisioning** page, click **Next**.

13. On the **Configure Extensions** page, click **Finish**.


## Create Run Profiles

Create run profiles for the  ADMA and MIMMA Connectors.

### Create run profiles for the ADMA connector

This table shows the five run profiles you will create for the ADMA connector:

| Name | Type |
| ---- | ---- |
| Profile1 | Full Import (Stage Only) |
| Profile2 | Full Synchronization |
| Profile3 | Delta Import (Stage Only) |
| Profile4 | Delta Synchronization |
| Profile5 | Export |

To create run profiles for the ADMA connector:

1. Open the Synchronization Service Manager and on the **Tools** menu, click **Management Agents**.

2. In the **Management Agents** list, select **ADMA**.

3. To open the Configure Run Profiles for dialog box, on the **Actions** menu, click **Configure Run Profiles**.

4. For each run profile in the table, complete the following steps:

    - To open the Configure Run Profile wizard, click **New Profile**.

    - In the **Name** box, type the profile name from the table, and click **Next**.

    - In the **Type** list, select the step type from the table, and then click **Next**.

    - Click **Finish** to create the run profile.

5. To close the Configure Run Profiles dialog box, click **OK**.

### Create run profiles for the MIMMA connector

This table shows the matching five run profiles for the MIMMA connector:

| Name | Type |
| -------- | -------- |
| Profile1 | Full Import (Stage Only) |
| Profile2 | Full Synchronization |
| Profile3 | Delta Import (Stage Only) |
| Profile4 | Delta Synchronization |
| Profile5 | Export |

To create run profiles for the MIMMA connector:

1. Open the Synchronization Service Manager and on the **Tools** menu, click **Management Agents**.

2. In the **Management Agents** list, select **MIMMA**.

3. To open the Configure Run Profiles for dialog box, on the **Actions** menu, click **Configure Run Profiles**.

4. For each run profile in the table, complete the following steps:

    - To open the Configure Run Profile wizard, click **New Profile**.

    - In the **Name** box, type the profile name from the table, and click **Next**.

    - In the **Type** list, select the step type from the table, and then click **Next**.

    - Click **Finish** to create the run profile.

5. To close the Configure Run Profiles dialog box, click **OK**.

## Configure the MIM Service

Using the MIM Portal, you will create the AD user inbound synchronization rule for MIM Service.

To create the AD user inbound synchronization rule:

1. On the MIM Portal home page, on the navigation bar, click **Administration**.

2. To open the Synchronization Rules page, click **Synchronization Rules**.

3. To open the Create Synchronization Rule wizard, in the toolbar, click **New**.

4. On the **General** tab, provide the following information, and then click **Next**:

    -   Display Name: AD User Inbound Synchronization Rule
    -   Data Flow Direction: Inbound

5. On the **Scope** tab, provide the following information, and then click **Next**:

    -   Metaverse Resource Type: person
    -   External System: ADMA
    -   External System Resource Type: person

6. On the **Relationship** tab, provide the following information, and then click **Next**:

    -   To configure the Relationship Criteria, select **ObjectSID** from the MetaverseObject:person(Attribute) list and from the ConnectedSystemObject:person(Attribute)list.

    -   Select **Create Resource in MIM**.

7. On the **Inbound Attribute Flow** page, provide the following information, and then click **Next**:

    | Flow Rule | Source | Destination |
    |-|-|-|
    |Rule 1|samAccountName|f|
    |Rule 2|displayName|displayName|
    |Rule 3|EmployeeType|EmployeeType|
    |Rule 4|givenName|givenName|
    |Rule 5|sn|lastName|
    |Rule 6|Manager|manager|
    |Rule 7|objectSID|ObjectSID|
    |Rule 8|"Contoso"|domain|

    For each row in this table, perform the following steps:

    - To open the Flow Definition dialog box, click **New Attribute Flow**.

    - On the **Source** tab, select the attribute shown for that row in the table.

    - On the **Destination** tab, select the attribute shown for that row in the table.

    - To apply the attribute flow configuration, click **OK**.

8. On the **Summary** tab, click **Submit**.

## Initialize the testing environment
There are four steps you need to take before you can test your MIM configuration with AD data:

### Enable Provisioning

1. Open the Synchronization Service Manager.

2. To open the Options dialog box, on the **Tools** menu, click **Options**

3. Select **Enable Synchronization Rule Provisioning**.

4. To close the Options dialog box, click **OK**.

### Initialize the MIMMA

Run a complete synchronization cycle on this connector. The complete cycle consists of the following run profiles:

- Full Import
- Full Synchronization
- Export
- Delta Import

Follow these steps to run each of the four run profiles.

1. Open the Synchronization Service Manager and, on the **Tools** menu, click **Management Agents**.

2. In the **Management Agents** list, select **MIMMA**.

3. To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

4. For each run profile listed above, complete the following steps:

    - To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

    - In the **Run profiles** list, select the run profile you want to run.

    - To start the run profile, click **OK**.

#### Configure attribute flow precedence

During the initialization of the MIM connector, the configured synchronization rules were brought into the metaverse.

Adjust the attribute flow precedence for the attributes contributed by this connector to ensure that attributes already in AD can flow into the metaverse and later also into the MIM Service database.

### Initialize the ADMA

To initialize the Active Directory connector, you need to run a full import and a full synchronization on it. The full import brings the existing objects from AD into the connector space. The full synchronization updates the synchronization rules to match those of the MIM connector.

1. Open the Synchronization Service Manager and in the **Tools** menu, click **Management Agents**.

2. In the **Management Agents** list, select **ADMA**.

3. To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

4. For each run profile listed above, complete the following steps:

    - To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

    - In the **Run profiles** list, select the run profile you want to run.

    - To start the run profile, click **OK**.

### Populate the MIM Service database

To populate the MIM Service database with the objects, you need to run a synchronization cycle on the MIMMA connector. The cycle consists of:

- Export
- Full Import
- Full Synchronization

Follow these steps to run each of the three run profiles.

1. Open the Synchronization Service Manager and click **Management Agents** on the **Tools** menu.

2. Select **MIMMA** in the **Management Agents** list.

3. Click **Run**  on the **Actions** menu to open the Run Management Agent dialog box.

4. For each run profile listed above, complete the following steps:

    - Click **Run** on the **Actions** menu to open the Run Management Agent dialog box.
    - Select the run profile you want to run from the **Run profiles** list.
    - Click **OK** to start the run profile.

>[!div class="step-by-step"]
[« MIM Service and Portal](install-mim-service-portal.md)
