---
title: Update Smartcard Status
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
---
# Update Smartcard Status
Updates the status of a smartcard.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
## Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid} 

### URL Parameters
Parameter | Description
---------|------------
reqid | Required. The Request identifier (MIM CM specific). 
scid | Required. The smartcard identifier (MIM CM specific). This is the "uuid" property in the [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) object.

### Request Headers
For common request headers, see [HTTP Request and Response Headers](CM%20REST%20API%20Service%20Details.md#HttpHeaders) in *CM REST API Service Details*.
### Request Body
The request body contains the following properties.

Property | Description
---------|-----------
status | The status to set the request to. For example, "Retired".


## Response
### Response Codes
Code  |Description  
---------|---------
200     | OK
204 | No content
403 | Forbidden
500 | Internal Error

### Response Headers
For common response headers, see [HTTP Request and Response Headers](CM%20REST%20API%20Service%20Details.md#HttpHeaders) in *CM REST API Service Details*.
### Response Body
On success, returns a JSON-Serialized [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) object with the following properties:

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

### Request 
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### Response
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
## See Also

- [Microsoft.Clm.Shared.Smartcards.Smartcard Class](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx))
