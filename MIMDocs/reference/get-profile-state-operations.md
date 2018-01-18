---
# required metadata

title: Get profile state operations | Microsoft Docs
description:
keywords:
author: msmbaldwin
ms.author: barclayn
manager: mbaldwin
ms.date: 09/26/2017
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get profile state operations
Gets a list of possible operations that can be performed by the current user on the specified profile. A request can then be initiated for any of the specified operations.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

### URL parameters

Parameter | Description
---------|------------
id | The identifier (GUID) of the profile or smartcard.

### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
None.

## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200 | OK
204 | No content
403 | Forbidden
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a list of possible operations that can be performed by the user on the smart card. The list can contain any number of the following operations: **OnlineUpdate**, **Renew**, **Recover**, **RecoverOnBehalf**, **Retire**, **Revoke**, and **Unblock**.

## Example
This section provides an example to get profile state operations for the current user.

### Example: Request

```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
