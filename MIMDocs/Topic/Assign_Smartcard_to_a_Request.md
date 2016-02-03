---
title: Assign Smartcard to a Request
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
---
# Assign Smartcard to a Request
Binds the specified smartcard to the specified request. Once bound, the request can only be executed with this card.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards 

###URL Parameters
none.
###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Request Body
The request body contains the following properties.

Property | Description
---------|-----------
requestid | The ID of the request that the smartcard should be bound to.
cardid | The cardid of the smartcard.
atr | The smart card answer-to-reset (ATR) string.


##Response
###Response Codes
Code  |Description  
---------|---------
201     | Created
204 | No Content
403 | Forbidden
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns a URI to the newly created smartcard object.
##Example

###Request
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###Response
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##See Also

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard Method (String, String, Request)](https://msdn.microsoft.com/en-us/library/windows/desktop/bb456812(v=vs.100%29.aspx)
