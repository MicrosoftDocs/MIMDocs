---
title: Get Smartcard Proposed PIN
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
---
# Get Smartcard Proposed PIN
Gets the server-generated user PIN.

**Note**: The server will set the PIN only if the profile template policy indicates that it should be done. Otherwise, the user should supply it.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin 

###URL Parameters
Parameter | Description
---------|------------
id | The smartcard identifier (MIM CM specific). Obtained from the Microsft.Clm.Shared.Smartcard object. 
###Query Parameters
Parameter | Description
---------|------------
atr | The card atr.
cardid | The card id.
challenge | A base-64 encoded string representing the challenge issued by the smartcard.

###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
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
For common response headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns a string that represents the PIN proposed by the server.

##Example

###Request 
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###Response 
```
HTTP/1.1 200 OK

... body coming soon
```       
