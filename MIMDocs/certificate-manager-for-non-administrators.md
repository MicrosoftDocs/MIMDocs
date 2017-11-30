﻿---
# required metadata

title: Microsoft Identity Manager Self-service smart card renewal without Administrator access | Microsoft Docs
description: Learn how to enroll smart cards for users without administrator access to their machines so they can use Certificate Manager.
keywords:
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Enroll smart cards for non-administrators
If a user isn’t a local administrator on their computer, they won’t be able to enroll a smart card on their own machines by default. The following procedure enables you to work around this limitation.

## Enabling smart card renewal for non-admins in MIM 2016 Certificate Manager

1.  **Unpack the appx file**

    Obtain a signing certificate. Follow the steps to [Sign Windows 8 applications using an internal PKI](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Stop when you get to “Sign the Application”. Name the exported pfx file. Export to a .cer file as well, and import it to the client using the cer file of the new signing certificate.

    Run the following to unpack the appx file:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Modify the configuration file**

    Rename the file named `CustomDataExample.xml custom.data`. The CM app will look for this file name.

    Edit the custom.data file and modify the following:

    1.  In the &lt;NonAdmin&gt; element, change the value of the Value attribute to "True"

    2.  Save the file and exit editor

    3.  Delete the file named AppxSignature.p7x

    4.  Edit the file named AppxManifest.xml

    5.  In the &lt;Identity&gt; element modify the value of the Publisher attribute to the subject of your signing certificate, e.g. "CN=ABCD"

        The subject here should be the same as the subject in the signing certificate you’re using to sign the app.

    6.  Save the file and exit editor.

3.  **Re-pack and sign the app package (appx file)**

    Run the following to pack and sign the the appx file:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    `signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplicate the profile template and adding the initial admin key to configure the MIM server:

    1.  Log into the CM portal as a user with administrative privileges.

    2.  Go to **Administration** &gt; **Manage Profile templates** and make sure that the box is checked next to profile template you created, then click on Copy a selected profile template.

    3.  Type the name of the profile template, add “nonAdmin” and click **OK**.

    4.  When the profile template general settings appear, scroll down all the way and under **Smart card Configuration**, click **Change Settings**.

    5.  Under **Admin key initial value (hex)** enter the default admin key: "010203040506070801020304050607080102030405060708"

    6.  Scroll down and click **OK**.

5.  **Create a non-admin account on the client machine**

    Non-admin users can't create the virtual smart card on the TPM, so you have to create it for them.

6.  **Create a virtual smart card using TpmVscMgr**

    Perform the following (still as the admin) to create an empty virtual smart card on a machine. This can be done through Intune, SCCM or group policies.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Install the CM app in the non-admin account**

8.  **Launch the CM app and enrolling for a virtual smart card**
