---
# required metadata

title: Policy Operations | Microsoft Docs
description:
keywords:
author: barclayyn
ms.author: barclayn
manager: mbaldwin
ms.date: 12/31/2016
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:
---


# Microsoft Identity Manager 2016 Portal Customization


>[!Warning]
Be sure to clear the browser cache when any CSS customizations are made.

In FIM 2010 R2 , you can customize selected elements of the password portals,
including the banner logo, string resources, and cascading style sheets.

In order to do this, a few things are required depending on the level of
customization. The following is a list of items that are involved in customizing
the FIM 2010 R2 password registration and reset portals.

-   Customizations Folder - This is the folder that FIM 2010 R2 will check prior
    to using the defaults. Each portal that will be customized requires a
    Customizations folder. Customizations should only be done in this folder
    because setup will not overwrite it on upgrades, change mode installs or
    repair mode installs.

-   Strings.resources - This is an XML-based file that allows you to modify the
    strings that appear in the portal. This file needs to reside in the
    Customizations folder.

-   Style.css – This is the cascading style sheet used by the portals for
    customization. This style sheet needs to be created and modified to change
    the logo or can be entirely replaced with your own customizations.

For detailed step-by-step instructions on customizing the password registration
and password reset portals see Test Lab Guide: Demonstrating FIM 2010 R2
Password Registration and Reset Portal Customization.

>[!WARGNING]
In order for FIM to recognize any customized changes, you must restart IIS by running iisreset.


## Creating the Customizations folder

On startup, FIM will look for the Strings.resources file in the Customizations
folder before using the defaults. You must create a Customizations folder under
the directory for the portal you wish to customize (i.e. Password Registration
Portal or Password Reset Portal). If you want to customize both portals then you
will need to create a Customizations folder under each of the following:

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password
    Registration Portal

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password
    Reset Portal

### To create the customization folder

1.  Navigate to the C:\\Program Files\\Microsoft Forefront Identity
    Manager\\2010\\Password Registration Portal folder.

2.  Create a folder named Customizations.

3.  Navigate back one level to the Password Reset Portal folder, and create a
    folder named Customizations.

## Customizing strings

Many of the strings in the portal UI can be customized by creating a
Strings.resources file and adding this file to the Customizations folder. You
will need to create a Strings.resources file for each portal that you wish to
customize.

### To customize strings

1.  Using notepad, copy the following code below into it and save it to the
    Customizations folder as Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Under the `<!-- Customizations begin here -->` section change the data name
    to match the strings that you wish to customize and enter the value for that
    string between the <value></value> tags. See the list below for the
    strings that can be customized and their default values.

>[!NOTE]
The **Strings.resources** file is language neutral. To create language specific customized strings, you must have that language pack installed, and save the file in the format Strings. `<language>-<culture>.resources` , for example Strings.en-us.resources.

The following is a list of portal strings that can be customized.

Portal Strings
--------------

| Name                                                     | Default Value                                                                                                                                                                                                                                                                                                                                         | Comment                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | About         |       |
| ButtonCancel                                             | Cancel                                                                                 |     |
| ButtonFinish                                             | Finish    |    |
| ButtonNext                                               | Next    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | Your session is no longer active. You can close the window, or you can restart by clicking the link below.         |      |
| CancelFinishedTitle                                      | Session Ended                                                              |      |
| ErrorDescription_3000                                    | An error has occurred. Please try again, and if the problem persists, contact your help desk or system administrator. (Error 3000)       |          |
| ErrorDescription_3001                                    | Ensure you enter your user name correctly.If you still cannot reset your password, please contact your      |           |
|                                                          | helpdesk for assistance. (Error 3001)   |                   |
| ErrorDescription_3002                                    | Your session has ended. Return to the home page to start again. (Error 3002)                                     |                |
| ErrorDescription_3003                                    | The current user account is not recognized by Forefront Identity Manager.Please contact your help desk or system administrator. (Error 3003)           |               |
| ErrorDescription_3004                                    | You are not authorized to register for password reset.Please contact your help desk or system administrator. (Error 3004)     |              |
| ErrorDescription_3005                                    | One or more answers that you provided do not match the answers which you provided during Password Registration.In order to reset your password, the answers that you provide now must match the answers that you provided when you registered.You can start again from the home page, or contact your help desk or system administrator. (Error 3005) |           |
| ErrorDescription_3006                                    | The password that you entered is incorrect.You must enter the correct password in order to register for Password Reset. (Error 3006)            |              |
| ErrorDescription_3007                                    | You are temporarily prohibited from resetting your password.Please try again later, or contact your help desk or system administrator for assistance. (Error 3007)     |         |
| ErrorDescription_3008                                    | An error has occurred. Please try again, and if the problem persists, contact your help desk or system administrator. (Error 3008)        |          |
| ErrorDescription_3009                                    | Your input contains text in a format that is not allowed. Try again with different input, or contact your help desk or system administrator. (Error 3009)     |             |
| ErrorDescription_3010_Registration                       | Scripting is not enabled on your browser. Enable scripting and return to the Password Registration home page, or contact your help desk for assistance.      |               |
| ErrorDescription_3010_Reset                              | Scripting is not enabled on your browser. Enable scripting and return to the Password Reset home page, or contact your help desk for assistance.     |            |
| ErrorDescription_3011                                    | This site uses cookies. Configure your browser to accept cookies and try again, or contact your help desk for assistance.          |                                           |
| ErrorDescription_3012                                    | The data you entered did not match the security code that was sent to you. You can try to reset your password again, or contact your help desk for assistance.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Unable to send a security code. Please contact your help desk for assistance.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Enter your user name in the correct format.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Enter a user name to continue.                                              |                         |
| ErrorMessagePasswordRequired                             | Enter a password.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Ensure both passwords match.                                |           |
| ErrorPageDefaultHeading                                  | Application Error                                            |               |
| ErrorPageServerTime                                      | Server time: {0:T}                     | {0} is the time at which the exception was caught. The 'T' causes the passed-in time to be formatted as a "long time." It will end up showing the hour, minute, and second, and possibly the AM/PM designation (depending on the current culture). |
| ErrorPageTitle                                           | Forefront Identity Management - Password Error                     |   |
| ErrorTitle_3000                                          | Error                                  |  |
| ErrorTitle_3001                                          | Access Denied                          |  |
| ErrorTitle_3002                                          | Session Ended                          |  |
| ErrorTitle_3003                                          | Unrecognized User                      |  |
| ErrorTitle_3004                                          | Unauthorized User                      |  |
| ErrorTitle_3005                                          | Answers Don't Match                    |  |
| ErrorTitle_3006                                          | Incorrect Password                     |  |  
| ErrorTitle_3007                                          | Access Denied Temporarily              |  |
| ErrorTitle_3008                                          | Communication Error                    |  |
| ErrorTitle_3009                                          | Prohibited Input                       |  |
| ErrorTitle_3010                                          | Browser Configuration Error            |  |
| ErrorTitle_3011                                          | Browser Configuration Error            |  |
| ErrorTitle_3012                                          | Verification failed                    |  |
| ErrorTitle_3013                                          | Unable to send security code   |  |
| FinalizeRegistrationHeading1                             | If you ever need to reset your password:        |   |
| FinalizeRegistrationSubHeading1                          | Go to the reset password portal   |   |
| FinalizeRegistrationSubHeading2                          | Verify your identity   |   |
| FinalizeRegistrationSubHeading3                          | Choose your new password    |     |
| FinishingDescription                                     | Choose Your New Password        |    |
| FinishingTitle                                           | Password Reset:      |     |
| GotoPortalPrefix                                         | Go to     |        |
| GotoPortalSuffix                                         | home page     |      |
| LabelTroubleshootingLinkText                             | View Details           |    |
| LoadingText                                              | Loading ...     |  |
| NoScriptTagErrorMessage                                  | Scripting is not enabled on your browser. Enable scripting and return to the home page, or contact your help desk for assistance.      |   |
| PasswordResetOperationGeneralErrorMessage                | Error while attempting to reset password.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | The password does not comply with your organization's password policies.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Error while resetting password, the user cannot change the password.    |   |
| PrivacyStatement                                         | Privacy Statement                                                      |    |
| RegistrationDescription                                  | Self-Service Password Registration     |    |
| RegistrationMission                                      | If you ever forget your password, you can reset it yourself without calling your help desk.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - Password Registration                                          |   |
| RegistrationSteps                                        | Click 'Next' to begin the registration process.   |      |
| RegistrationSuccessDescription                           | You are now registered        |   |
| RegistrationSuccessTitle                                 | Completed:   |    |
| RegistrationWelcomeTitle                                 | Password Registration:    | |
| ResetDescription                                         | Self-Service Password Reset  |    |
| ResetEnterNamePrompt                                     | Please enter your user name below     |     |
| ResetEnterPassword                                       | Enter a new password:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Examples:  |    |
| ResetPageTitle                                           | Forefront Identity Management - Password Reset       |     |
| ResetReenterPassword                                     | Re-enter the password:    | |
| ResetSuccessDescription                                  | Your password has been reset    |    |
| ResetSuccessTitle                                        | Success:                                |    |
| ResetUseNewPassword                                      | You can now use your new password to log in.     |      |
| ResetUsernameTextFormat                                  | (Resetting password for {0})       | {0} is the user's logon             |
| ResetWelcomeTitle                                        | Password Reset:     |      |
| TroubleshootingEmailSubject                              | FIM request processing error details     |       |
| TroubleshootingLabelAttributes                           | Attributes:    |    |
| TroubleshootingLabelCloseButton                          | Close       |    |
| TroubleshootingLabelCopyToClipboard                      | Copy to Clipboard     |     |
| TroubleshootingLabelCorrelationId                        | Correlation Id:      |          |
| TroubleshootingLabelDetails                              | Details:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | The information has been copied to the clipboard.           |    |
| TroubleshootingLabelRequestId                            | Request Id:                  |    |
| TroubleshootingLabelSendEmail                            | Send Information by Email                            |    |
| TroubleshootingLabelSource                               | Reason:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | View Request Details        |              |                                                    
| TroubleshootingLinkText                                  | Troubleshooting Information          |                  | |



The following is a list of Authentication Gate strings that can be customized.

Authentication Gate Strings
---------------------------

| Name                                          | Default Value                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | Email address:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | The email address field cannot be empty.     |          |
| OTPEmailRegistrationFooterReadOnly            | To update your email address, follow the process defined by your organization or call your help desk.                   |          |
| OTPEmailRegistrationFooterReadWrite           | The email address is stored by your organization in Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Email Address Verification   |          |
| OTPEmailRegistrationHeaderReadOnly            | If you ever need to reset your password, a verification security code will be sent to your email. If the email address shown below is not correct, you will need to update this in order to use self-service password reset. |          |
| OTPEmailRegistrationHeaderReadWrite           | Enter your email address below. If you ever need to reset your password, a verification code will be sent to your email.                 |          |
| OTPEmailResetGateTitle                        | Verify Your Identity: Email         |          |
| OTPEmailResetHeader                           | Enter your security code below. A security code was sent to the email address registered with this organization.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | The specified value does not match the expected format.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | The security code field cannot be empty.        |          |
| OTPResetVerificationLabel                     | Security Code:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | To update your mobile phone number, follow the process defined by your organization or call your help desk.   ||
| OTPSmsRegistrationFooterReadWrite                     | The mobile phone number is stored by your organization in Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Mobile Phone Verification                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | If you ever need to reset your password, a verification security code will be sent to your mobile phone. If the mobile phone number shown below is not correct, you will need to update this in order to use self-service password reset. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Enter your mobile phone number below. If you ever need to reset your password, a verification code will be sent to your mobile phone.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | The mobile phone number field cannot be empty.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobile Phone:                    |   |
| OTPSmsResetGateTitle                                  | Verify Your Identity: Mobile Phone         |   |
| OTPSmsResetHeader                                     | Enter your security code below. A security code was sent to the mobile phone registered with this organization.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Enter your current password below, then click 'Next'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Enter your current password.                  |   |
| PasswordGateGateTitle                                 | Your Current Password               |   |
| PasswordGatePasswordLabelText                         | Password:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(logged in as: `<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | You must answer at least {0} questions.        |   |
| QAGateIncorrectAnswer                                 | Your answers are not correct.       |   |
| QAGatePrivacyNotice                                   | The responses you provide are stored by your organization in Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | You must answer at least {0} questions to register.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | One or more answers do not comply with policy.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | This answer does not comply with policy.      |   |
| QAGateRegistrationTitle                               | Register Your Answers         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | You must answer {0} of the following {1} questions.   |   |
| QAGateResetTitle                                      | Verify Your Identity: Submit Your Answers                                  |   |



## Customizing the logo banner

The default banner on the portal pages can be customized for your organization.
To customize the logo banner:

1.  Create your custom banners and save them as .png files. The files should
    meet the following recommendations:

 - Size: 490 X 50 pixels.

 - Bit depth: 32

2.  Copy the files to the \\Customizations folder in each portal that you want
    to customize.

3.  Create a Style.css file in each folder. Have it point to the Customizations
    folder and the new logo.. You may change the logo name if applicable (i.e.
    /Customizations/contosologo.png). The code should look like the following:

   **Example 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  If you are using Internet Explorer 6.0, you will need to provide an
    alternate non-transparent logo, and add the following code to Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Example 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

If you are using Internet Explorer 6.0, you will need to provide an alternate non-transparent logo, and add the following code to Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## Customizing image for SmartPhones

You can customize the image for SmartPhones by using the following. To customize the SmartPhone image:

1.  Create your image and save them as .png files. The files should meet the
    following recommendations:

    - Size: 190 X 50 pixels.
    - Bit depth: 32

2.  Copy the files to the \\Customizations folder in each portal that you want
    to customize.

2.  Create a Style.css file in each folder. Have it point to the Customizations
    folder and the new logo.. You may change the logo name if applicable (i.e.
    /Customizations/contosologo.png). The code should look like the following:

  **Example 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## Customizing style sheets

You can modify the layout and style of the password portals by using a
customized cascading style sheet (CSS). To use a customized CSS:

1.  Create your customized CSS files and save them as Style.css.

2.  Copy the files to the \\Customizations folder in each portal that you want
    to customize.

The following is a basic example of a Style.css file:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
In order for FIM to recognize any customized changes, you must restart IIS by running iisreset.                                                                                                                                                                                       

The following is a more advanced example of a Style.css file. This file provides smartphone and ipad specific information for displaying the portals on these devices.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## Common Customization Issues

The following table is a list of known common issues that can occur with
upgrading the FIM Service and Portal. This table also includes the resolutions
for these issue

|Issue |Resolution                                                                    |
|------|------------------------------|
| I made a string customization but it wasn’t reflected in the UI         | String customizations in strings.resources always require iisreset         |
| After making a strings.resources change, I don’t see any of my string changes anymore     | Strings.resources format is probably malformed and hence is ignored by the portal. Check the event log under Windows Logs – Application and Services Logs – Forefront Identity Manager                        |
| The first time I added Style.css, I did not see my style changes in the portal            | The very first time you introduce a Style.css file, you need to do an iisreset     |
| New styles are added/modified in Style.css but changes are not seen in the browser      | Clear the browser cache and refresh the page <br/> Check the CSS syntax    |
| I directly changed the contents of the CSS folder in path_to_sspr_portal\\css\\\*.css or the banner logo in path_to_sspr_portal\\images\\fimlogo.png and lost these changes on upgrade | You should never change these files to begin with. Only use the Customization folder as a means to provide a banner logo and CSS style customizations in Style.css. Customizations folder is deliberately not overwritten by major upgrades <br/><br/>Do not try to use tools like ILSpy and Reflector to change strings in the portal assemblies. Use strings.resources to override default strings. The assemblies are replaced on upgrade  |
| Banner logo is not displayed in the portals / I still see the FIM logo     | The image name/path in Style.css is not valid or the browser cache was not cleared          |
| Banner logo looks ugly in IE6       | You’ll need to provide a non-transparent image for IE6 and a special accompanying style in style.css        |
