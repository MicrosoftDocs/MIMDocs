---
# required metadata

title: Assign Smart card to a Request | Microsoft Docs
description:
keywords:
author: msmbaldwin
ms.author: barclayn
manager: mbaldwin
ms.date: 09/26/2017
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
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

# Assign Smart card to a Request
Binds the specified smart card to the specified request. Once bound, the request can only be executed with this card.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###URL Parameters
none.
###Request Headers
For common request headers, see [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API Service Details*.
###Request Body
The request body contains the following properties.

Property | Description
---------|-----------
requestid | The ID of the request that the smart card should be bound to.
cardid | The cardid of the smart card.
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
For common response headers, see [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API Service Details*.
###Response Body
On success, returns a URI to the newly created smart card object.
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

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard Method (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
