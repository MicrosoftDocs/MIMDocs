---
title: Certificate Manager for Software Certificates
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
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
author: Kgremban
---
# Certificate Manager for Software Certificates
To enroll and renew software certificates you don’t have to be an administrator and you don’t need a virtual smart card. It’s worth noting that at some point you will be prompted to allow a certificate operation and this is normal.

## Creating a software certificate Profile Template in MIM 2016 Certificate Manager

1.  Create a template for the certificate that you will request for the virtual smart card. Open the mmc.

2.  Click **File**, and then click **Add/Remove Snap-in**.

3.  In the available snap-ins list, click **Certificate Templates**, and then click **Add**.

4.  **Certificate Templates** is now located under Console Root in the MMC. Double-click it to view all the available certificate templates.

5.  Right-click the **User template**, and click **Duplicate Template**.

6.  On the **Compatibility** tab under Certification Authority Select Windows Server 2008 and under Certificate Recipient select Windows 8.1 / Windows Server 2012 R2.

    1.  On the **General** tab, in the display name field type **Archived Certificate Template**.

    2.  b.	On the **Request Handling** tab

        1.  Set the **Purpose** to Signature and encryption.

        2.  Check **Include symmetric algorithms allowed by the subject**.

        3.  If you want to archive the key, check **Archive subject’s encryption private key**.

        4.  Under Do the following… select **Prompt the user during enrollment**.

    3.  On the **Cryptography** tab

        1.  Under Provider Category select **Key Storage Provider**

        2.  Select **Requests can use any provider available on the subject's computer**.

    4.  On the **Security** tab, add the security group that you want to give **Enroll** access to. For example, if you want to give access to all users, select the **Authenticated** users group, and then select **Enroll** permissions for them.

    5.  On the **Subject Name** tab

        1.  Uncheck **Include e-mail name in subject name**.

        2.  Under **Include this information in alternate subject name**, uncheck **Email name**.

    6.  Click **OK** to finalize your changes and create the new template. Your new template should now appear in the list of Certificate Templates.

    7.  Select **File**, then click **Add/Remove Snap-in** to add the Certification Authority snap-in to your MMC console. When asked which computer you want to manage, select **Local Computer**.

    8.  In the left pane of the MMC, expand **Certification Authority (Local)**, and then expand your CA within the Certification Authority list.

    9. Right-click **Certificate Templates**, click **New**, and then click **Certificate Template** to Issue.

    10. From the list, select the new template that you just created (**Archived Certificate Template**), and then click **OK**.

## Create the Profile Template

1.  Log into the CM portal as a user with administrative privileges.

2.  Go to **Administration &gt; Manage Profile templates** and make sure that the box is checked next to **MIM CM Sample Smart Card Logon Profile Template** and then click on **Copy a selected profile template**.

3.  Type the name of the profile template and click **OK**.

4.  In the next screen, click **Add new certificate template** and make sure to check the box next to the CA name.

5.  Check the box next to the name of the Archived Software Certificate and click **Add**.

6.  Remove the User template by checking the box next to it and then clicking **Delete selected certificate templates** and then **OK**.

7.  Click **Change general settings**.

8.  Check the boxes to the left of **Generate encryption keys on the server** and click on **OK**. On the left pane, click on **Recover Policy**.

9. Click **Change general settings**.

10. If you want to reissue archived certificates, check the boxes to the left of **Reissue archived certificates** and click on **OK**.

11. If you are using the Virtual Smart Card CM, you have to disable data collection items because it doesn't work with data collection on. Disable data collection for each and every policy by clicking on the policy in the left pane, and then unchecking the box next to **Sample data item** and then click **Delete data collection items**. Then click **OK**.

