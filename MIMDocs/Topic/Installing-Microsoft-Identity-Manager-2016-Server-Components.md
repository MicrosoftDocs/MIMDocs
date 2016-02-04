---
title: Installing Microsoft Identity Manager 2016 Server Components
ms.custom: 
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 901e3634-38b1-421e-a995-049e763c7672
author: Kgremban
---
# Installing Microsoft Identity Manager 2016 Server Components

1.  On the corpidm server you are using for  identity management, log in as *contoso\Administrator*. Administrator rights are used to install **MIM Sync, Service, and Portal** in this environment.

2.  Unpack the MIM installation package or mount the MIM image DVD.

## Install MIM 2016 Synchronization Service

1.  In the unpacked MIM installation folder, navigate to the **Synchronization Service** folder.

2.  Run the **MIM Synchronization Service installer**. Follow the guidelines of the installer and complete the installation.

3.  In the welcome screen – click **Next**.

    ![](../Image/MIM_Install1.png)

4.  Review the license terms and if you accept them, click **Next**.

5.  In the feature selection screen click **Next**.

    ![](../Image/MIM_Install2.png)

6.  In the Sync database configuration screen, select:

    1.  The SQL Server is located on: **This computer**.

    2.  The SQL Server instance is: **The default instance**.

        ![](../Image/MIM_Install3.png)

7.  Configure the Sync Service Account according to the account you created earlier:

    1.  Service account: *MIMSync*

    2.  Password: *Pass@word1*

    3.  Service Account Domain or local computer name: *contoso*

        ![](../Image/MIM_Install4.png)

8.  Provide MIM Sync installer with the relevant security groups:

    1.  Administrator = *contoso\MIMSyncAdmins*

    2.  Operator= *contoso\MIMSyncOperators*

    3.  Joiner = *contoso\MIMSyncJoiners*

    4.  Connector Browse = *contoso\MIMSyncBrowse*

    5.  WMI Password Management= *contoso\MIMSyncPasswordReset*

9. In the security settings screen, check Enable firewall rules for inbound RPC communications, and click **Next**.

    ![](../Image/MIM_Install5.png)

10. Click **Install** to begin the installation of MIM  Sync.

    1.  A warning concerning the MIM Sync service account may appear – click **OK**.

    2.  MIM Sync will now be installed.

        ![](../Image/MIM_Install6.png)

    3.  A notice on creating a backup for the encryption key will be shown – click OK, then select a folder to store the encryption key backup.

        ![](../Image/MIM_Install7.png)

    4.  When the installer successfully completes the installation, click **Finish**.

        ![](../Image/MIM_Install8.png)

    5.  You will be prompted to log off and log on for group membership changes to take effect. Click **Yes** to logoff.

## Install MIM Service and Portal

1.  Run the **MIM Service and Portal installer** from the unpacked **Service and Portal** sub-folder.

2.  In the welcome screen, click **Next**.

3.  Read the End-User License Agreement, and if you accept the license terms, click **Next**.

4.  In the MIM Customer Experience Improvement Program screen, click **Next**.

5.  For this deployment, when selecting component features, it is necessary to include  the MIM Service (except for MIM Reporting) and MIM Portal features. You can also select the MIM Password Reset Portal and MIM Password Reset Portal.

6.  When configuring common services and the MIM database connection, specify “Create a new database”.

    ![](../Image/MIM_Install10.png)

7.  When configuring a mail server connection, set the mail server to  your Exchange server. If you do not have a mail server configured, specify localhost as the mail server name, and uncheck the top two checkboxes.

    ![](../Image/MIM_Install11.png)

8.  Specify that you want to generate a new self-signed certificate, or select the relevant certificate.

9. Specify the Service Account name to use, for example, *MIMService*, and the Service Account password, for example, *Pass@word1* (the password in the first step above), your Service Account domain, for example, contoso and the Service Email Account, for example contoso.

    ![](../Image/MIM_Install12.png)

10. Note that a warning may appear that the Service Account is not secure in its current configuration.

11. Accept the defaults for the Synchronization Server location, and specify the MIM Management Agent account as *contoso\MIMsync*.

    ![](../Image/MIM_Install13.png)

12. Specify *CorpIDM (this computer's name)* as MIM Service server address for the MIM Portal.

13. Specify *http://CorpIDM.contoso.local:82* as the SharePoint site collection URL.

14. Specify *http://CorpIDM.contoso.local:8080* as the Password Registration URL.

15. Specify *http://CorpIDM.contoso.local:8088* as the Password Reset URL.

16. Select the checkbox to open ports 5725 and 5726 in the firewall, and the checkbox to grant all authenticated users access to MIM Portal.

17. In the MIM Password Registration Portal configuration screen

    1.  Specify the service account name for SSPR Registration as *contoso\MIMSSPR* and its password to *Pass@word1*.

    2.  Specify  CORPIDM as the Host Name for MIM Password Registration, and set the port to **8080**. Enable the opening the port in firewall.

        ![](../Image/MIM_Install14.png)

    3.  A warning will appear – read it and click **Next**.

18. In the next MIM Password Registration Portal configuration screen, specify  *http://CorpIDM.contoso.local* as the MIM Service Server Address for the Password Registration Portal.

19. In the MIM Password Reset Portal configuration screen

    1.  State the service account name for SSPR Registration as *Domain\MIMSSPRService* and its password to *Pass@word1*.

    2.  Specify  *CorpIDname  http://CorpIDname.domain.local* as the Host Name for MIM Password Registration, port **8088**. Enable the opening the port in firewall.

        ![](../Image/MIM_Install15.png)

    3.  A warning will appear – read it and click **Next**.

20. In the next MIM Password Registration Portal configuration screen, specify *CorpIDname  http://CorpIDname.domain.local* as the MIM Service Server Address for the Password Registration Portal.

21. When all pre-installation definitions are ready, click **Install** to begin installing the selected **Service and Portal** components.

    ![](../Image/MIM_Install16.png)

22. After installation completes, verify that the MIM Portal is active.

    1.  Launch Internet Explorer and connect to the MIM Portal on  *http://corpidm.contoso.local:82/identitymanagement*

    2.  Note that there may be a short delay on the first visit to this page.

    3.  If necessary, authenticate as *contoso\Administrator* to Internet Explorer.

    4.  In Internet Explorer, open the **Internet Options**, change to the **security** tab, and add the site to the **Local intranet** zone if it is not already there.  Close the **Internet Options** dialog.

    5.  Enable users to view their own entry in MIM.

        1.  Using Internet Explorer, in **MIM Portal**, click on **Management Policy Rules**.

        2.  Search for the management policy rule:

            User management: Users can read attributes of their own.

        3.  Select this management policy rule, uncheck Policy is disabled,

        4.  Click **OK** and then click **Submit**.

    6.  Verify that the firewall allows incoming connections to TCP port 5725 and 5726.

        1.  Launch **Administrative Tools » Windows Firewall** with **Advanced Security**

        2.  Click on **Inbound Rules**.

        3.  Verify that the two following rules are listed:

            -   Forefront Identity Manager Service (STS).

            -   Forefront Identity Manager Service (Webservice).

        4.  Complete the wizard and close the **Windows Firewall** application.

        5.  Launch **Control Panel »** Network and Internet **»** View network status and tasks.

        6.  Verify that there is an active Network listed as contoso.local as a Domain network.

        7.  Close **Control Panel**.

> [!NOTE]
> Optional: At this point you can install MIM add-ins and extensions.

## Configure MIM Sync to Synchronize from Active Directory to MIM Service
By default, MIM Sync does not have any connectors configured.   A typical first step is to use MIM Sync to populate the FIM Service database with existing Active Directory accounts.  For this, you will use the MIM Sync Service application.

### Create the MIM MA
The MIM MA is a connector for MIM Sync to the MIM Service. To create this connector, you use the Create Management Agent wizard.

When you configure a MIM management agent, you need to specify a user account. This document uses mimma as name for this account.

> [!CAUTION]
> The account you use for your MIM management agent must be the same account as the one you have specified during the installation of MIM Service.

To create the MIM MA

-   Open the Synchronization Service Manager.

-   To open the Create Management Agent wizard, on the Actions menu, click **Create**.

-   On the Create Management Agent page, provide the following settings, and then click **Next**.

    -   Management agent for: FIM Service management agent

    -   Name: MIMMA

-   On the Connect to Database page, provide the following settings, and then click Next

    -   Server: localhost

    -   Database: FIMService

    -   FIM Service base address: http://localhost:5725

    -   Authentication mode: Windows integrated authentication

    -   User name: mimma

    -   Password: Pass@word

    -   Domain: contoso

-   On the Selected Object Types page, verify that the object types that are listed below are selected, and then click Next

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   Group

-   On the Selected Attributes page, verify that all listed attributes are selected, and then click Next.

-   On the Configure Connector Filter page, click Next.

-   On the Configure Object Type Mappings page, add the following mapping, and then click Next

    -   In the Data Source Object Type list, select Person.

    -   To open the Mapping dialog box, click Add Mapping.

    -   In the Metaverse object type list, select person.

    -   To close the Mapping dialog box, click OK.

-   On the Configure Attribute Flow page, apply the following attribute flow mappings, and then click Next

    ||||
    |-|-|-|
    |Flow Direction|Data Source Attribute|Metaverse Attribute|
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

-   Select Person as the Data source object type.

    -   Select person as the Metaverse object type.

    -   Select Direct as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the Flow Direction shown for that row in the table.

        -   Select the Data source attribute shown for that row in the table.

        -   Select the metaverse attribute shown for that row in the table.

        -   To apply the flow mapping, click New.

    -   Select Group as the data source type and as the metaverse object type.

    -   Select Direct as the Mapping Type.

    -   For each row in the following table, complete the following steps:

        -   Select the Flow Direction shown for that row in the table.

        -   Select the Data source attribute shown for that row in the table.

        -   Select the metaverse attribute shown for that row in the table.

        -   To apply the flow mapping, click New.

    ||||
    |-|-|-|
    |Flow Direction|Data Source Attribute|Metaverse Attribute|
    |Export|AccountName|accountName|
    |Export|DisplayName|displayName|
    |Export|Domain|domain|
    |Export|Scope|scope|
    |Export|Type|type|
    |Export|Member|member|
    |Export|MembershipLocked|membershipLocked|
    |Export|MembershipAddWorkflow|membershipAddWorkflow|
    |Export|Manager|manager|

-   On the Configure Deprovisioning page, click Next

-   To create the management agent, on the Configure Extensions page, click Finish.

### Create the AD MA
The AD MA is a connector for AD DS. To create this connector, you use the Create Management Agent wizard.

-   To open the Create Management Agent wizard, on the Actions menu, click Create

-   On the Create Management Agent page, provide the following settings, and then click Next:

    -   Management agent for: Active Directory Domain Services

    -   Name: ADMA

-   On the Connect to Active Directory Forest page, provide the following settings, and then click Next:

    -   Forest name: contoso.local

    -   User name: administrator

    -   Password : &lt;the account’s password&gt;

    -   Domain: contoso

-   On the Configure Directory Partitions page, provide the following settings, and then click Next:

    -   In the Select directory partitions list, select DC=CONTOSO, DC=local.

    -   To open the Select Containers dialog box, click Containers.

    -   If you wish to change the container to only have MIM manage objects in a particular container, click the DC=CONTOSO,DC=local node, and then click the node for the container of interest.

    -   To close the Select Containers dialog box, click  OK

-   On the Configure Provisioning Hierarchy page, click Next.

-   On the Select Object Types page, provide the following settings, and then click Next

-   In the Object types list, select user and group.

-   On the Select Attributes page, provide the following settings, and then click Next

-   Select Show All.

    In the Attributes list, select the following attributes:

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

-   On the Configure Connector Filter page, click Next.

-   On the Configure Join and Projection Rues page, click Next.

-   On the Configure Attribute Flow page, click Next.

-   On the Configure Deprovisioning page, click Next.

-   On the Configure Extensions page, click Finish.

### Create Run Profiles
Create run profiles for the  ADMA and MIMMA Connectors.

#### Create run profiles for the ADMA connector
Create the following profiles for the ADMA connector:

-   Profile1: Full Import (Stage Only)

-   Profile2: Full Synchronization

-   Profile3: Delta Import (Stage Only)

-   Profile4: Delta Synchronization

-   Profile5: Export

###### To create run profiles for the ADMA connector:

1.  Open the Synchronization Service Manager and, on the Tools menu, click Management Agents.

2.  In the Management Agents list, select ADMA.

3.  To open the Configure Run Profiles for dialog box, on the Actions menu, click Configure Run Profiles.

4.  For each run profile in the table immediately above this procedure, complete the following steps:

    -   To open the Configure Run Profile wizard, click New Profile.

    -   In the Name box, type the profile name shown in the table, and click Next.

    -   In the Type list, select the step type shown in the table, and then click Next.

    -   Click Finish to create the run profile.

5.  To close the Configure Run Profiles dialog box, click OK.

#### Create run profiles for the MIMMA connector
Create the following profiles for the MIMMA connector:

-   Profile1: Full Import (Stage Only)

-   Profile2: Full Synchronization

-   Profile3: Delta Import (Stage Only)

-   Profile4: Delta Synchronization

-   Profile5: Export

###### To create run profiles for the MIMMA connector:

1.  Open the Synchronization Service Manager and, on the Tools menu, click Management Agents.

2.  In the Management Agents list, select MIMMA.

3.  To open the Configure Run Profiles for dialog box, on the Actions menu, click Configure Run Profiles.

4.  For each run profile in the table immediately above this procedure, complete the following steps:

    -   To open the Configure Run Profile wizard, click New Profile.

    -   In the Name box, type the profile name shown in the table, and click Next.

    -   In the Type list, select the step type shown in the table, and then click Next.

    -   Click Finish to create the run profile.

5.  To close the Configure Run Profiles dialog box, click OK.

### Configuring the MIM Service
For the scenario in this document, you perform the following steps in the MIM Service, using the MIM Portal:

-   Create the AD user inbound synchronization rule

##### To create the AD user inbound synchronization rule

1.  On the MIM portal home page, on the navigation bar, click Administration.

2.  To open the Synchronization Rules page, click Synchronization Rules.

3.  To open the Create Synchronization Rule wizard, in the toolbar, click New.

4.  On the General tab, provide the following information, and then click Next:

    -   Display Name: AD User Inbound Synchronization Rule

    -   Data Flow Direction: Inbound

5.  On the Scope tab, provide the following information, and then click Next:

    -   Metaverse Resource Type: person

    -   External System: ADMA

    -   External System Resource Type: person

6.  On the Relationship tab, provide the following information, and then click Next

    -   To configure the Relationship Criteria, select objectSID from the MetaverseObject:person(Attribute) list and ObjectSID from the ConnectedSystemObject:person(Attribute)list.

    -   Select Create Resource In FIM.

7.  On the Inbound Attribute Flow page, provide the following information, and then click Next:

    ||||
    |-|-|-|
    |Flow Rule|Source|Destination|
    |Rule 1|samAccountName|f|
    |Rule 2|displayName|displayName|
    |Rule 3|EmployeeType|EmployeeType|
    |Rule 4|givenName|givenName|
    |Rule 5|sn|lastName|
    |Rule 6|Manager|manager|
    |Rule 7|objectSID|ObjectSID|
    |Rule 8|"Contoso"|domain|
    For each row in this table, perform the following steps:

    -   To open the Flow Definition dialog box, click New Attribute Flow

    -   On the Source tab, select the attribute shown for that row in the table.

    -   On the Destination tab, select the attribute shown for that row in the table.

    -   To apply the attribute flow configuration, click OK.

8.  On the Summary tab, click Submit.

### Initializing the testing environment
Before you can test your configuration with your AD data, you need to initialize the configuration. The following steps are part of this process:

-   Enabling provisioning

-   Initializing the MIMMA

-   Configuring attribute flow precedence

-   Initializing the ADMA

##### To enable Provisioning

1.  Open the Synchronization Service Manager.

2.  To open the Options dialog box, on the Tools menu, click Options

3.  Select Enable Synchronization Rule Provisioning.

4.  To close the Options dialog box, click OK.

##### To initialize the MIMMA

1.  run a complete synchronization cycle on this connector. The complete cycle consists of the following run profile runs:

    -   Full Import

    -   Full Synchronization

    -   Export

    -   Delta Import

2.  Open the Synchronization Service Manager and on the Tools menu, click Management Agents.

3.  In the Management Agents list, select MIMMA

4.  To open the Run Management Agent dialog box, on the Actions menu, click Run.

5.  For each row in the table immediately above this procedure, complete the following steps:

    -   To open the Run Management Agent dialog box, on the Actions menu, click Run.

    -   In the Run profiles list, select the run profile shown for that row in the table.

6.  To start the run profile, click OK.

##### Configuring attribute flow precedence

1.  During the initialization of the MIM connector, the configured synchronization rules have been brought into the metaverse.

2.  Adjust the attribute flow precedence for the attributes contributed by this connector to ensure that attributes already in AD can flow into the metaverse and later also into the MIM Service database.

##### Initializing the ADMA

1.  To initialize the Active Directory connector, you need to run a full import and a full synchronization on it. The full import is required to bring the existing objects from AD into the connector space. The full synchronization is required because the synchronization rules have changed by projecting the new synchronization rules from the MIM connector space into the metaverse.

    -   Step 1: Run Full Import

    -   Step 2: Run Full Synchronization

2.  Open the Synchronization Service Manager and in the Tools menu, click Management Agents.

3.  In the Management Agents list, select ADMA.

4.  To open the Run Management Agent dialog box, on the Actions menu, click Run.

5.  For each row in the table immediately above this procedure, complete the following steps:

    -   To open the Run Management Agent dialog box, on the Actions menu, click Run.

    -   In the Run profiles list, select the run profile shown for that row in the table.

6.  To start the run profile, click OK.

##### Populating the MIM Service database

1.  To populate the MIM Service database with the objects, you need to run a synchronization cycle on the MIMMA connector. The cycle consists of the following run profile runs:

    -   1: Export

    -   2: Full Import

    -   3: Full Sync

2.  Open the Synchronization Service Manager and on the Tools menu, click Management Agents.

3.  In the Management Agents list, select MIMMA

4.  To open the Run Management Agent dialog box, on the Actions menu, click Run.

5.  For each row in the table immediately above this procedure, complete the following steps:

    -   To open the Run Management Agent dialog box, on the Actions menu, click Run.

    -   In the Run profiles list, select the run profile shown for that row in the table.

6.  To start the run profile, click OK.

