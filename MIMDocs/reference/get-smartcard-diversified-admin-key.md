---
# required metadata

title: Get smart card diversified admin key | Microsoft Docs
description: Using the MIM CM REST API GET command to find the diversified admin key for a specified smart card.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get smart card diversified admin key
Gets the diversified admin key for the specified smart card.

>[!IMPORTANT]
>The admin key should be diversified only when the profile template policy indicates that it should be.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### URL parameters

Parameter | Description
---------|------------
reqid | Required. The request identifier that's specific to Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Required. The smart card identifier that's specific to MIM CM. The scid is obtained from the [Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object.

### Query parameters

Parameter | Description
---------|------------
atr | Optional. The smart card answer-to-reset (ATR) string.
cardid | Required. The card ID.

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
On success, returns a byte BLOB representing the diversified admin key.

## Example
This section provides an example to get the diversified admin key for a smart card.

### Example: Request

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## See also

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard method (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
