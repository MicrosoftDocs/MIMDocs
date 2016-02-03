---
title: Get Smartcard or Profile Certificates
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
---
# Get Smartcard or Profile Certificates
Gets a list of certificates associated with the particular smartcard or software profile.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificateManagement/api/v1.0/smartcards/{id}/certificates 

###URL Parameters
Parameter | Description
---------|------------
id | The identifier (GUID) of the profile or smartcard.

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
On success, returns a list of JSON-serialized [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.certificates.x509clmcertificate(v=vs.100%29.aspx) objects with the following properties:

Name | Description
-----|------------
ArchivedOnCa | A Boolean value that indicates if the certificate is archived on the certification authority (CA). 
CertificateType | The type of the certificate. 
IsKeyHistory | A Boolean value that indicates if the certificate is a key history certificate. 
SerialNumber | The certificate's serial number. 
TemplateCommonName | The certificate template common name for the certificate. 
Thumbprint | The certificate's thumbprint. 

##Example

###Request 
```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```
###Response 
```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       
##See Also

- [Microsoft.Clm.Provision.FindOperations.FindCertificates Method](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.findoperations.findcertificates(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.Certificates.X509ClmCertificate Class](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.certificates.x509clmcertificate(v=vs.100%29.aspx) 
