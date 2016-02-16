---
title: Privileged Access Management REST API Reference
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
---
# Privileged Access Management REST API Reference
Microsoft Identity Manager (MIM) 2016 adds a new scenario called Privileged Access Management (PAM). PAM enables an organization to have more control over the access rights of high privileged user accounts, such as system or service administrators, to sensitive resources. PAM controls high privilege account access by providing limited time access rights, just in time (JIT), when the access rights are needed. 

A user can ask MIM Service for privileged access rights (elevation) in one of two ways:

- By using the PAM REST API 
- By using the PAM PowerShell New-PAMRequest cmdlet

The topics in this guide describe the PAM REST API. For more information about using the PowerShell cmdlet see The Test Lab Guide: Demonstrating Privileged Access Management using Microsoft Identity Manager, available on the connect site.

##PAM REST API Resources and Operations
The PAM REST API operates on the following resources
- **PAM Role**: A PAM role associates a collection of users with a collection of access rights. The access rights are defined by reference to security groups.  Every PAM role has a list of user accounts, called candidates, that are entitled to elevate to the PAM role. You can perform the following operations on PAM roles:

    - [Get PAM Roles](Get-PAM-Roles.md)

- **PAM Request**: A user who wants to elevate to a PAM role access rights has to submit a PAM request and get the approval for the request in order to elevate. The PAM Request object tracks the lifecycle of this request in the MIM Service. You can perform the following operations on PAM requests:

    - [Create PAM Request](Create-PAM-Request.md)
    - [Get PAM Requests](Get-PAM-Requests.md)
    - [Close PAM Request](Close-PAM-Request.md)

- **Pending PAM Request**: Used to approve or reject PAM requests that have been submitted by users. You can perform the following operations on pending PAM requests:

    - [Get Pending PAM Requests](Get-Pending-PAM-Requests.md)
    - [Approve or Reject a Pending PAM Request](Approve-or-Reject-a-Pending-PAM-Request.md)

- **PAM Session**: When using the PAM REST API, the client (for example, a web browser) has a session with the PAM REST API endpoint. In this session the client is authenticated to the REST API endpoint. You can perform the following operations on PAM sessions:

     - [Get PAM Session Info](Get-PAM-Session-Info.md)

For more detailed information about the service, see [PAM REST API Service Details](PAM-REST-API-Service-Details.md)

##PAM Sample Portal on GitHub
One way to learn how to use the PAM REST API is by using the PAM sample portal, an example web application which uses the API. You can find the code for the PAM Sample portal in the [PAM sample repository on GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). You can learn how to deploy the sample portal in the PAM Test Lab Guide.
