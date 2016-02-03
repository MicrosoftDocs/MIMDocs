---
title: FIM Issues and Solutions
ms.custom: na
ms.prod: windows-server-2012
ms.reviewer: na
ms.service: active-directory
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 107775f8-2bf6-490a-86bf-653171b26c66
author: Kgremban
---
# FIM Issues and Solutions
## Forefront Identity Manager Issues and Solutions
### Issue List
[User Attributes in Request Notifications](#issue_-user-attributes-in-request-notifications)
[Another Issue](#another-issue)

### Issue: User Attributes in Request Notifications
When someone makes a request to add a user to a group, the group owner receives a FIM-generated email asking to approve that request. The recipient can’t tell whether they should approve the request because by default FIM sends just the new member’s display name. Display names are not unique, for example "Administrator".   More attributes in the request in the email are needed such as the domain of the user account.

### Solution: Add attribute values
In the approval and notification emails, the attribute values of the change being requested can be sent in the email message body by including in the email template in:

-  the field `[//RequestParameter/AllChangesAuthorizationTable]` if it is an *authorization* workflow or 
- the field `[//RequestParameter/AllChangesActionTable]` if it is in an *action* workflow.

When the requestor requests to add values to a reference-valued attribute (e.g., adding a reference to a Person as a value of the ExplicitMember attribute of the Group), typically only one row is included in the table, and that row includes the display name of the requested linked object (e.g., the person's name).   
 
If different or additional attributes of the referenced object should be included in the email, the system names of those attributes can be included within this field in the email template.  For example, `[//RequestParameter/AllChangesAuthorizationTable,”Domain”,”AccountName”,”DisplayName”]`
 
![fim-request-email](/Image/fim-request-email.jpg)

## Issue: Another Issue
blah blah blah
## Solution: Another Solution
blah de blah blah