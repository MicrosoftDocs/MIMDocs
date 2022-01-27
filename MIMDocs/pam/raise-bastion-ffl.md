---
# required metadata

title: Raise the bastion forest functional level | Microsoft Docs
description: Raise a Privileged Access Management deployment that started with Windows Server 2012 R2 functional level to the Windows Server 2016 functional level.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/27/2021
ms.topic: article
ms.prod: microsoft-identity-manager

---
# Raise the bastion forest functional level

As MIM 2016 went to general availability prior to Windows Server 2016, MIM PAM was designed to operate with either the Windows Server 2012 R2 or Windows Server 2016 functional level of the bastion forest.

## Checking if you need to raise the functional level

1. Make sure you are signed in into the server hosting MIM Service as a MIM administrator.
2. Launch PowerShell.
3. Type the following command.

    ```powershell
    (Get-PAMConfiguration).ForestFunctionality
    ```

    If the output is `DS_BEHAVIOR_WIN2012R2`, then you need to raise the functional level, as outlined in the rest of this article.  If the behavior is `DS_BEHAVIOR_WIN2015`, then you do not need to raise the functional level.

## Step 1 - Upgrade Windows Server and MIM software

## Step 2 - Raise the domain functional level

## Step 3 - Update the domain controller configuration

## Step 4 - Update the MIM PAM configuration


