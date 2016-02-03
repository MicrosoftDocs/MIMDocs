---
title: Get Certificate Request Generation Options
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
---
# Get Certificate Request Generation Options
Gets parameters for client-side certificate request generation. 

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions 

###URL Parameters
Parameter | Description
--------|--------------
requestid| Required. The GUID identifier of the MIM CM request for which the certificate request generation parameters are to be retrieved.

###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Request Body
none.


##Response
###Response Codes
Code  |Description  
---------|---------
200 | OK
204 | No Content
403 | Forbidden
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns list of CertificateRequestGenerationOptions objects. Each CertificateRequestGenerationOptions object corresponds to a single certificate request that the client has to generate and has the following properties:

Property| Description
--------|-----------
Exportable | A value that specifies whether the private key created for the request can be exported. 
FriendlyName | The display name of the enrolled certificate. 
HashAlgorithmName | The hash algorithm used when creating the certificate request signature. 
KeyAlgorithmName | The public key algorithm. 
KeyProtectionLevel | The level of strong key protection. 
KeySize | The size, in bits, of the private key to be generated. 
KeyStorageProviderNames | A list of acceptable key storage providers (KSPs) that can be used to generate the private key. In the case that the first KSP cannot be used to generate the cert request, any of the specified KSPs may be used until one succeeds.
KeyUsages | The operation that can be performed by the private key created for this certificate request. The default value is Signing. 
Subject | The subject name. 

**Note**: More information about these properties is available in the [Windows.Security.Cryptography.Certificates.CertificateRequestProperties class](https://msdn.microsoft.com/en-us/library/windows/apps/br212079.aspx) description, but be aware that there is not a one-to-one correspondence between this class and CertificateRequestGenerationOptions objects.

##Example

###Request
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###Response
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
