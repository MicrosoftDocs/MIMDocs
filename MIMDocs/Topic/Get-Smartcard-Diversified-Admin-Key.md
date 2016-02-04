---
title: Get Smartcard Diversified Admin Key
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
---
# Get Smartcard Diversified Admin Key
Gets the diversified admin key for the specified smartcard.

**Note**: The admin key only needs to be diversified if the Profile template policy indicates that it should be.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey 

###URL Parameters
Parameter | Description
---------|------------
reqid | Required. The request identifier (MIM CM specific). 
scid | Required. The smartcard identifier (MIM CM specific). Obtained from the [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) object.
###Query Parameters
Parameter | Description
---------|------------
atr | Optional. The smart card answer-to-reset (ATR) string.
cardid | Required. The card id.

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
On success, returns a byte BLOB representing the diversified admin key.

##Example

###Request 
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###Response 
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##See ALso

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard Method (String, String, Request) Method](https://msdn.microsoft.com/en-us/library/windows/desktop/bb456812(v=vs.100%29.aspx)
