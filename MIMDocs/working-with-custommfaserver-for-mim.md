---

title: Use an MFA provider via an API to activate PAM or in SSPR scenario | Microsoft Docs
description: Set up Custom MFA API as a second layer of security when your users activate roles in Privileged Access Management and use Self Service Password Reset.
keywords:
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager



---

# Use a custom multi-factor authentication provider via an API during PAM role activation or in SSPR

MIM customers have two options for multi-factor authentication in the SSPR and PAM scenarios:

 - Use a custom one-time-password delivery provider, which is applicable only in the MIM SSPR scenario and documented in guide to [Configure Self-Service Password Reset with OTP SMS Gate](/previous-versions/mim/hh824692(v=ws.10))
 - Use a custom multi-factor authentication telephony provider. This is applicable in both the MIM SSPR and PAM scenarios, described in this article

This article outlines how to use MIM with a custom multi-factor authentication provider, via an API and an integration SDK developed by the customer.  

## Prerequisites

In order to use a custom multi-factor authentication provider API with MIM, you need:

- Phone numbers for all candidate users
- MIM hotfix 4.5.202.0 or later - see [version history](reference/version-history.md) for announcements
- MIM Service configured for SSPR or PAM

## Approach using custom multi-factor authentication code

### Step 1: Ensure MIM Service is at version 4.5.202.0 or later

Download and install MIM hotfix [4.5.202.0](https://support.microsoft.com/help/4346632/hotfix-rollup-package-build-4-5-202-0-is-available-for-microsoft) or a later version.

### Step 2: Create a DLL which implements the IPhoneServiceProvider interface

The DLL must include a class, which implements three methods:

- `InitiateCall`: The MIM Service will invoke this method. The service passes the phone number and request ID as parameters.  The method must return a `PhoneCallStatus` value of `Pending`, `Success` or `Failed`.
- `GetCallStatus`: If an earlier call to `initiateCall` returned `Pending`, the MIM Service will invoke this method. This method also returns `PhoneCallStatus` value of `Pending`, `Success` or `Failed`.
- `GetFailureMessage`: If a previous invocation of `InitiateCall` or `GetCallStatus` returned `Failed`, the MIM Service will invoke this method. This method returns a diagnostic message.

The implementations of these methods must be thread safe, and furthermore the implementation of the `GetCallStatus` and `GetFailureMessage` must not assume that they will be called by the same thread as an earlier call to `InitiateCall`.

Store the DLL in the `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` directory.

Sample code, which can be compiled using Visual Studio 2010 or later.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### Step 3: Backup the MfaSettings.xml located in the "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### Step 4: Edit the MfaSettings.xml file

Update or clear the following lines:

- Remove/Clear all configuration entries lines 

- Update or add the following lines to the following to MfaSettings.xml with your custom phone provider <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### Step 5: Restart MIM Service

After the service has restarted, use SSPR and/or PAM to validate functionality with the custom identity provider.

> [!NOTE] 
> To revert setting replace MfaSettings.xml with your backup file in step 3


## Next Steps

- [MIM version release history](./reference/version-history.md)
