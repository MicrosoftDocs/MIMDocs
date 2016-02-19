---
title: Getting Started with Privileged Access Management
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
ms.assetid: ebf516a1-0a1d-4f28-959b-8de4a8f5d4e1
author: Kgremban
---
# Getting Started with Privileged Access Management

## In This Guide
This guide contains instructions for demonstrating Microsoft Identity Manager and Active Directory Domain Services features for privileged access management for administration across forests.

## Background
In many sophisticated cyber-attacks against various enterprises worldwide, the attackers often focus their attention on gaining administrative privileges.  In particular, they may use “Pass-the-Hash”, spear-phishing, or other techniques to gain the access rights of a user who has administrative privileges across a domain or forest.  These attacks are exacerbated because many users have permanent administrative privileges associated with their Active Directory account.  If an attacker compromises any one of those user’s accounts and then can log in or run a program as that user, the attacker then has administrative privileges. Organizations concerned about the possibility for insider attack and constraining access with IT outsourcing are also motivated for improving controls or users with highly privileged access.  The document Best Practices for Securing Active Directory highlights the need to “Eliminate permanent membership in highly privileged groups” and “Implement controls to grant temporary membership in privileged groups when needed”, and this guide demonstrates how these controls can be achieved using Windows Server Active Directory and Microsoft Identity Manager.

The solution architecture for privileged access management (PAM) is based on two concepts:

-   Control by managing a user’s access, not their credential, and leverage Active Directory groups to provide that access

-   Extract and isolate administrative accounts from existing Active Directory forests

This solution is primarily focused on domain accounts in which an end user will be authenticating and authorized to act as a role or application administrator of a particular collection of systems or across multiple services which rely upon Kerberos (or ADFS) with Active Directory-hosted security groups.  This test lab guide is not intended to cover scenarios for service accounts, local administrator accounts, shared accounts, or non-AD environments.

