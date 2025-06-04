---
# required metadata

title: Update smart card status | Microsoft Docs
description: Updating smart card status from the MIM CM REST API.
keywords:
author: billmath
ms.author: billmath
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Update smart card status
Updates the status of a smart card.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### URL parameters

Parameter | Description
---------|------------
reqid | Required. The request identifier that's specific to Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Required. The smart card identifier that's specific to MIM CM. The value corresponds to the "uuid" field in the [Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object.

### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
The request body contains the following properties:

Property | Description
---------|-----------
status | The status to set the request to, such as "Retired."

## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200     | OK
204 | No content
403 | Forbidden
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a JSON-Serialized [Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) object with the following properties:

Name | Description
-----|-----------
AssignedUserUuid | The identifier of the user to whom the smart card is assigned.
Atr | The smart card answer-to-reset (ATR) string for the card that is currently being initialized.
Comment | The comment that describes the smart card.
Flags | The flags that describe the smart card.
Middleware | The middleware for the smart card.
ParentSmartcardUuid | The identifier of the old smart card that the smart card has replaced.
PermanentSmartcardUuid | The identifier of the permanent smart card that is associated with the smart card.
PrimarySmartcardUuid | The identifier of the primary smart card.
ProfileTemplateUuid | The identifier of the profile template that contains the policies and settings that govern the smart card.
ProfileTemplateVersion | The version of the profile template at the time that the smart card profile was created.
SerialNumber | The smart card's serial number.
Status | The status of the smart card.
Uuid | The smart card profile's identifier.

## Example
This section provides an example to update the status of a smart card.

### Example: Request

```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       

## See also

- [Microsoft.Clm.Shared.Smartcards.Smartcard class](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
