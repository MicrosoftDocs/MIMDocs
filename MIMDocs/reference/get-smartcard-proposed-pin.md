---
# required metadata

title: Get smart card proposed PIN | Microsoft Docs
description: Using the MIM CM REST API GET command to retrieve the server-generated user PIN.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 01/27/2023
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid: ced93932-9912-4b32-9586-ada69b38a796

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get smart card proposed PIN
Gets the server-generated user PIN.

>[!IMPORTANT]
>The server only sets the PIN if the profile template policy indicates that it should be done. Otherwise, the user should supply the PIN.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### URL parameters

Parameter | Description
---------|------------
id | The smart card ID that's specific to Microsoft Identity Manager (MIM) Certificate Management (CM). The ID is obtained from the Microsft.Clm.Shared.Smartcard object.

### Query parameters

Parameter | Description
---------|------------
atr | The smart card answer-to-reset (ATR) string.
cardid | The smart card ID.
challenge | A base-64 encoded string representing the challenge that's issued by the smart card.

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
On success, returns a string that represents the PIN that's proposed by the server.

## Example
This section provides an example to get the server-generated user PIN.

### Example: Request

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

... body coming soon
```       
