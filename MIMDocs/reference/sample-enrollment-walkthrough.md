---
# required metadata

title: Sample enrollment walkthrough | Microsoft Docs
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
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Sample enrollment walkthrough
This article shows the steps required to perform a virtual smart card self-service enrollment. It shows an auto-approved request with a user-set PIN.

1. The client authenticates the user, then requests a list of profile templates that the authenticated user can enroll for:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. The client displays the resulting list to the user. The user selects a vSC (virtual smart card) profile template with name “Virtual Smartcard VPN” and UUID `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1`.

3. The client retrieves the enrollment policy of the selected profile template by using the UUID returned in the previous step:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. The client analyzes the **DataCollection** field in the returned policy, noting that a single data collection item entitled “Request Reason” appears. The client also notes that the **collectComments** flag is set to false, so it doesn’t prompt the user to enter any.

5. After the user has entered the reason for requiring the certificates, the client creates a request:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. The server successfully creates the request and returns the URI of the request to the client:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5`.

7. The client retrieves the request object by calling the returned URI:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. The client verifies that the **state** property in the request is set to approved. Request execution may begin.

9. The client examines the request to see if there is a smart card already associated with the request by analyzing the contents of the **newsmartcarduuid** parameter.

10.	Since it only contains an empty GUID, the client must either use an existing card not already in use by MIM CM, or create one if the profile template is being configured for virtual smart cards.

11.	Since the latter has been indicated to the client through the initial query for enrollable profile templates (step 1), the client must now create a virtual smart card device.

12.	The client retrieves the smart card policy from the profile template. It uses the UUID of the template that was selected in step 3:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13.	The smart card policy contains the default admin key for the card in the **DefaultAdminKeyHex** property. When creating the smart card, the client must set the initial admin key of the smart card to this key.  
14.	Upon creating the smart card device, the client must assign it to the request:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. The server responds to the client with a URI to the newly created smart card object: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644`.

16.	The client retrieves the smart card object:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17.	By checking the value of the **diversifyadminkey** flag in the smart card policy obtained in step 12, the client knows it has to diversify the admin key.

18.	The client retrieves the proposed admin key:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19.	The client must authenticate to the card as admin in order to set the admin key. To do this, the client requests an authentication challenge from the smart card and submits it to the server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    The server sends the challenge response in the HTTP response body. Locate the hexadecimal challenge string, convert to base64, and pass it as a parameter in the URL. The URL returns another response, which must be converted back to hexadecimal format.

20.	The server sends the challenge response in the HTTP response body and the client uses it to diversify the admin key.

21.	The client notes that the user must provide their desired pin by inspecting the **UserPinOption** field in the smart card policy.

22.	After the user has entered their desired pin into a dialog, the client performs a challenge-response authentication as outlined in step 19, with the only difference being that the diversified flag should now be set to true:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23.	The client uses the response received from the server to set the desired user pin.

24.	The client is now ready to generate certificate requests. It queries the profile template certificate generation parameters to determine how the keys/requests should be generated.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25.	The server responds with a single JSON-serialized **CertificateRequestGenerationOptions** object. The object includes parameters for the exportability of the key, certificate friendly name, hashing algorithm, key algorithm, key size, and so on. The client uses these parameters to generate a certificate request.

26.	The single certificate request is submitted to the server. The client could additionally specify a PFX password that should be used to decrypt any PFX blobs when the certificate template specifies archiving the certificates on the CA, that is, the CA generates the key pair and then sends it to the client. The client may also choose to add some comments.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27.	After a few seconds, the server responds with a single, JSON-serialized **Microsoft.Clm.Shared.Certificate** object. By checking the **isPkcs7** flag, the client learns that this response is not a PFX blob. The client extracts the blob by base64 decoding the pkcs7 string and installs it.

28.	The client marks the request as completed.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
