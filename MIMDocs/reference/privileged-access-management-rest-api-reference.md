---
# required metadata

title: Privileged Access Management REST API reference | Microsoft Docs
description: List of resources for using the MIM PAM REST API to manage privileged user accounts.
keywords:
author: billmath
ms.author: billmath
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Privileged Access Management REST API reference
Microsoft Identity Manager (MIM) 2016 adds a new scenario called Privileged Access Management (PAM). PAM enables an organization to have more control over the access rights of high privileged user accounts, such as system or service administrators, to sensitive resources. PAM controls high privilege account access by providing limited time access rights, just in time (JIT), when the access rights are needed.

A user can ask MIM Service for privileged access rights (elevation) in one of two ways:

- By using the PAM REST API.
- By using the PAM PowerShell New-PAMRequest cmdlet.

The topics in this guide describe the PAM REST API. For more information about using the PowerShell cmdlet, see _The Test Lab Guide: Demonstrating Privileged Access Management using Microsoft Identity Manager_, available on the connect site.

## PAM REST API resources and operations
The PAM REST API operates on the following resources:
- **PAM role**: A PAM role associates a collection of users with a collection of access rights. The access rights are defined by reference to security groups.  Every PAM role has a list of user accounts, called candidates, that are entitled to elevate to the PAM role. You can perform the following operations on PAM roles:

    - [Get PAM Roles](privileged-access-management-get-roles.md)

- **PAM request**: A user who wants to elevate to PAM role access rights has to submit a PAM request and get approval for the request to elevate. The PAM Request object tracks the lifecycle of this request in the MIM Service. You can perform the following operations on PAM requests:

    - [Create PAM Request](privileged-access-management-create-request.md)
    - [Get PAM Requests](privileged-access-management-get-requests.md)
    - [Close PAM Request](privileged-access-management-close-request.md)

- **Pending PAM request**: Used to approve or reject PAM requests that have been submitted by users. You can perform the following operations on pending PAM requests:

    - [Get Pending PAM Requests](privileged-access-management-get-pending-requests.md)
    - [Approve or Reject a Pending PAM Request](privileged-access-management-approve-reject-pending-request.md)

- **PAM session**: When using the PAM REST API, the client (for example, a web browser) has a session with the PAM REST API endpoint. In this session, the client is authenticated to the REST API endpoint. You can perform the following operations on PAM sessions:

     - [Get PAM Session Info](privileged-access-management-get-session-info.md)

For more detailed information about the service, see [PAM REST API Service Details](privileged-access-management-rest-api-service-details.md).

## PAM sample portal on GitHub
One way to learn how to use the PAM REST API is by using the PAM sample portal, an example web application that uses the API. You can find the code for the PAM Sample portal in the [PAM sample repository on GitHub](https://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). You can learn how to deploy the sample portal in the PAM Test Lab Guide.
