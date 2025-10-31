---
# required metadata

title: Assign a smart card to a request | Microsoft Docs
description: Binding a smart card to a specified request.
keywords:
author: henrymbuguakiarie
ms.author: henrymbuguakiarie
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Assign a smart card to a request
Binds the specified smart card to the specified request. After a smart card is bound, the request can only be executed with the specified card.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

### URL parameters
None.

### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
The request body contains the following properties:

Property | Description
---------|-----------
requestid | The ID of the request to bind to the smart card.
cardid | The cardid of the smart card.
atr | The smart card answer-to-reset (ATR) string.


## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
201 | Created
204 | No Content
403 | Forbidden
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a URI to the newly created smart card object.

## Example
This section provides an example to bind a smart card.

### Example: Request

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### Example: Response

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## See also

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard method (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
