---
title: Get Smartcard Data
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
---
# Get Smartcard Data
Gets a list of a user’s smartcard profiles with a list of possible operations that can be performed by the current user. A request can then be initiated for any of the specified operations.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}
 

###URL Parameters
Property| Description
---------|--------
smartcarduuid | Optional. The smartcard UUID as denoted by MIM CM. This is the “uuid” field in the [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) object.

###Query Parameters
Property| Description
---------|--------
cardid | Optional. The smartcard UUID as denoted by MIM CM. This is the “uuid” field in the [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) object.


###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM%20REST%20API%20Service%20Details.md#HttpHeaders) in *CM REST API Service Details*.
###Request Body
none

##Response
###Response Codes
Code  |Description  
---------|---------
200     | OK
204 | No content
403 | Forbidden
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](CM%20REST%20API%20Service%20Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
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

##Example

###Request 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###Response 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###Request 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
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
##See Also

-[Microsoft.Clm.Shared.Smartcards.Smartcard Class](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx)
