---
title: Get Smartcard Authentication Response
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
---
# Get Smartcard Authentication Response
Gets the response to a Base CSP authentication challenge.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse 

###URL Parameters
Parameter | Description
---------|------------
reqid | Required. The Request identifier (MIM CM specific). 
scid | Required. The smartcard identifier (MIM CM specific). Obtained from the [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) object.
###Query Parameters
Parameter | Description
---------|------------
atr | Optional. The smart card answer-to-reset (ATR) string.
cardid | Required. The card id.
challenge | Required. A base-64 encoded string representing the challenge issued by the smartcard.
diversified | Required. A Boolean flag denoting weather the smartcard admin key was diversified.


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
On success, returns a byte BLOB that represents the challenge response.

##Example

###Request 
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###Response 
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##See Also

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse Method](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.executeoperations.getbasecspresponse(v=vs.100%29.aspx)
