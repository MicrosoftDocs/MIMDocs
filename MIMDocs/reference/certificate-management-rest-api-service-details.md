---
# required metadata

title: Certificate Management REST API service details | Microsoft Docs
description:
keywords:
author: msmbaldwin
ms.author: barclayn
manager: mbaldwin
ms.date: 09/26/2017
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Certificate Management REST API service details
The following sections describe details of the Microsoft Identity Manager (MIM) Certificate Management (CM) REST API.

## Architecture 
MIM CM REST API calls are handled by controllers. The following table shows the full list of controllers and samples of the context in which they can be used.


|                  Controller                   |                                                                                                                                                           Sample route                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificateDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificateRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            CertificatesController             |                                                                                                                /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates                                                                                                                 |
|             OperationsController              |                                                                                                                  /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations                                                                                                                   |
|              PoliciesController               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              ProfilesController               |                                                                                                                                                     /api/v1.0/profiles/{id}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequestsController               |                                                                                                                                         /api/v1.0/requests/{id} <br/> /api/v1.0/requests                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## HTTP request and response headers
HTTP requests sent to the API should include the following headers (this list is not exhaustive):

Header | Description
-------|------------
Authorization | Required. The contents depend on the authentication method. The method is configurable and can be based on Windows Integrated Authentication (WIA)) or Active Directory Federation Services (ADFS).
Content-Type | Required if the request has a body. Must be `application/json`.
Content-Length | Required if the request has a body. 
Cookie | The session cookie. May be required depending on authentication method.


HTTP responses include the following headers (this list is  not exhaustive):

Header | Description
-------|------------
Content-Type | The API always returns `application/json`.
Content-Length | The length of the request body, if present, in bytes.


## API versioning 
The current version of the CM REST API is 1.0. The version is specified in the segment directly following the `/api` segment in the URI: `/api/v1.0`. The version number changes when major modifications are made to the API interface.


## Enable the API 
The `<ClmConfiguration>` section of the Web.config file has been extended with a new key:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

This key determines whether the CM REST API is exposed to clients. If the key is set to "false", the route mapping for the API is not performed on application startup. Subsequent requests to API endpoints return an HTTP 404 error code. The key is set to “disabled” by default.

>[!NOTE]
>After changing this value to "true," remember to run **iisreset** on the server.

## Enable tracing and logging 
The MIM CM REST API emits trace data for each HTTP request sent to it. You can set the verbosity level of the trace information emitted by setting the following configuration value:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## Error handling and troubleshooting 
When exceptions occur while processing a request, the MIM CM REST API returns an HTTP status code to the web client. For common errors, the API returns an appropriate HTTP status code and an error code. 

Unhandled exceptions are converted to an `HttpResponseException` with HTTP Status Code 500 (“Internal Error”) and traced in both the event log and the MIM CM trace file. Each unhandled exception is written to the event log with a corresponding correlation ID. The correlation ID is also sent to the consumer of the API in the error message. To troubleshoot the error, an administrator can search the event log for the corresponding correlation ID and error details.

>[!NOTE]
>Stack traces that correspond to errors, which are generated as a result of consuming the MIM CM REST API, aren't sent back to the client. The traces aren't sent back to the client due to security concerns.
