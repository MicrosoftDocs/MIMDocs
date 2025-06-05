---
# required metadata

title: Approve or reject a pending PAM request | Microsoft Docs
description: Using the PAM REST API POST command to respond to pending PAM role requests.
keywords:
author: billmath
ms.author: billmath
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Approve or reject a pending PAM request
Used by a privileged account to approve, close, or reject a request to elevate to a PAM role.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

### URL parameters

Parameter | Description
----------|-----------
approvalId | The identifier (GUID) of the approval object in PAM, specified as `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`.

### Query parameters

Parameter | Description
----------|--------------
v | Optional. The API version. If not included, the current (most recently released) version of the API is used. For more information, see [Versioning in PAM REST API service details](privileged-access-management-rest-api-service-details.md#versioning).


### Request Headers
For common request headers, see [HTTP request and response headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API service details*.

### Request body
None.

## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200 | OK
401 | Unauthorized
403 | Forbidden
408 | Request Timeout   
500 | Internal Server Error
503 | Service Unavailable

### Response headers
For common request headers, see [HTTP request and response headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API service details*.

### Response body
None.

## Example
This section provides an example to approve a request to elevate to a PAM role.

### Example: Request

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK
```       
