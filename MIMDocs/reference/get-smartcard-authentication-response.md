---
# required metadata

title: Get smart card authentication response | Microsoft Docs
description: Using the MIM CM REST API GET command to retrieve the response to a base CSP authentication challenge.
keywords:
author: billmath
ms.author: billmath
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get smart card authentication response
Gets the response to a base cryptographic service provider (CSP) authentication challenge.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

### URL parameters

Parameter | Description
---------|------------
reqid | Required. The request identifier that's specific to Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Required. The smart card identifier that's specific to MIM CM. The scid is obtained from the [Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object.

### Query parameters

Parameter | Description
---------|------------
atr | Optional. The smart card answer-to-reset (ATR) string.
cardid | Required. The smart card ID.
challenge | Required. A base-64 encoded string representing the challenge that's issued by the smart card.
diversified | Required. A Boolean flag denoting whether the smart card admin key was diversified.

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
On success, returns a byte BLOB that represents the challenge response.

## Example
This section provides an example to get the response to a base CSP authentication challenge.

### Example: Request

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       

## See also

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse method](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
