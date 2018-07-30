---
# required metadata

title: Get certificate request generation options | Microsoft Docs
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get certificate request generation options
Gets parameters for client-side certificate request generation.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

| Method |                                       Request URL                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions |

### URL parameters

Parameter | Description
--------|--------------
requestid| Required. The GUID identifier of the MIM CM request for which the certificate request generation parameters are to be retrieved.

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
204 | No Content
403 | Forbidden
500 | Internal Error

### Response headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a list of CertificateRequestGenerationOptions objects. Each CertificateRequestGenerationOptions object corresponds to a single certificate request that the client has to generate. Each object has the following properties:

Property| Description
--------|-----------
Exportable | A value that specifies whether the private key created for the request can be exported.
FriendlyName | The display name of the enrolled certificate.
HashAlgorithmName | The hash algorithm used when creating the certificate request signature.
KeyAlgorithmName | The public key algorithm.
KeyProtectionLevel | The level of strong key protection.
KeySize | The size, in bits, of the private key to be generated.
KeyStorageProviderNames | A list of acceptable key storage providers (KSPs) that can be used to generate the private key. When the first KSP can't be used to generate the certificate request, any of the specified KSPs can be used until one succeeds.
KeyUsages | The operation that can be performed by the private key created for this certificate request. The default value is Signing.
Subject | The subject name.

>[!NOTE]
>More information about these properties is available in the [Windows.Security.Cryptography.Certificates.CertificateRequestProperties class](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) description. Keep in mind that there's not a one-to-one correspondence between this class and CertificateRequestGenerationOptions objects.
>

## Example
This section provides an example to get the generation options for a certificate request.

### Example: Request

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### Example: Response

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
