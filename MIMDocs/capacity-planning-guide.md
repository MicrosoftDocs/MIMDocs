---
# required metadata

title: Capacity planning guide | Microsoft Docs
description: Use this guide to understand the variables that should be considered before deploying MIM 2016, including load levels and policy decisions.
keywords:
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/21/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Capacity planning guide

Microsoft Identity Manager (MIM) lets you create, update, and remove user accounts throughout your organization. It also gives end users the ability to manage their own accounts self-service features. Even in a small environment, all these actions can add up quickly.

Before you get started with MIM use this guide, along with test environments, to understand the appropriate scope for your deployment. This article walks through many common factors that you should take into consideration. Since every deployment is unique, however, testing your scenarios in a lab is still the best way to determine the appropriate servers, hardware, or topologies for your needs.

If you aren't familiar with MIM 2016 and its components yet, get more details about [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) before continuing.

## Overview
There are a number of variables that can affect the overall capacity and performance of your Microsoft Identity Manager deployment. The ways in which you physically deploy the MIM components (topology), as well as the hardware on which those components run, are important factors in determining the performance and capacity that you can expect from your MIM deployment. The number and complexity of the MIM policy configuration objects may be less obvious, but they are still significant factors to consider when planning for capacity. Finally, the expected scale of the deployment, as well as the load that is expected to be placed on it, are typically more obvious factors that affect performance and capacity.

The main factors that affect the capacity and performance that can be expected from a MIM 2016 deployment are discussed in the following table.

| Design Factor | Considerations |
| ------------- | -------------- |
| Topology | The distribution of the MIM services among computers on the network. |
| Hardware | The physical hardware and any virtualized hardware specifications that you are running for each MIM component. This includes CPU, memory, network adapter, and hard drive configuration. |
| MIM policy configuration objects | The number and type of MIM policy configuration objects, which includes sets, Management Policy Rules (MPRs), and workflows. |
| Scale | The number of users, groups, calculated groups, and custom object types, such as computers to be managed by MIM 2016. Also, consider the complexity of dynamic groups, and be sure to factor in group nesting. |
| Load | Frequency of use. For example, how often you expect new groups or users to be created, passwords to be reset, or the portal to be visited in a given time period. Note that the load may vary during the course of an hour, day, week, or year. Depending on the component, you may choose to design for peak load or average load. |


## Hosting Microsoft Identity Manager components

The components of Microsoft Identity Manager don't have to be located on the same computer. Thinking about these components, and the physical or virtual machines that will host them, is an important part of capacity planning.

Hardware factors can affect the performance of the MIM environment. For example:
- What is the physical disk configuration for the computer running the MIM 2016 Service SQL Database? The number of spindles that make up the disk configuration or the distribution of log and data files can affect the performance of the system significantly.

Also, think about the external factors in your configuration. For example:
- If you are using a SAN as the MIM 2016 Service database configuration, what other applications are sharing the SAN? These applications may affect database performance if they compete for the shared disk resources on the SAN.


## Users and groups
The number of users and groups in your environment is a typical consideration when you think about the scale of a deployment. However, there are several other related considerations that you should also factor into planning.

- Can users create groups? If so, you should consider estimating how users creating new groups will affect the growth of groups in your environment.

- Will dynamic groups be deployed? Figure out how many and what types of dynamic groups to expect in your environment.


## Expected load levels
You should also consider the type of load that will be placed on the MIM components. This information can probably be estimated by looking at current applications in your environment. Some relevant questions to consider include the following:

- How often do you expect a request to join or leave a group?

- How often do you expect a user to create a static or dynamic group?

- How many non-user-driven operations do you expect, such as the synchronization of changes from external systems? Ensure that you account for load that is generated by synchronizing identity information with external systems.

- What kind of scenarios do you plan to deploy? Different scenarios will contribute to different load patterns. For example, client computers that have the MIM 2016 client installed periodically validate if registration is required at sign-in, which increases your system load.

- Do you expect large variations in load levels, from normal to peak load? For example, there tends to be a lot of password resets after holiday periods. Make sure that you work your system maintenance and synchronization schedules outside the anticipated usage peaks. When you think of capacity planning, ensure that you factor in peak load periods.


## Policy configuration objects

Microsoft Identity Manager policy configuration objects include the MPRs, sets, workflows, and synchronization rules for a particular deployment. MIM deployments are unique to each customer because policy configuration changes to fit the needs of each deployment. Key performance considerations related to MIM policy configuration objects include the following:

- **Sets** Every operation in the system must be evaluated against existing set memberships and updates that cause changes in set membership. For example, a simple change, such as change to the building number of an individual’s office, may not have a huge impact. However, other changes might have a cascading impact, such as the change of a manager, which may affect multiple objects at multiple levels.

- **Management Policy Rules** MPRs manage access control rules and trigger workflows. As you create MPRs, you may find that you need to increase the number of sets so that you can capture various object transition states. These additional sets may trigger additional workflows, with each workflow mapping to unique requests in the system. This then becomes another item to include when you plan capacity.

MIM policy configuration also includes decisions about provisioning in your environment. Make sure you think about the following:

- Will you be provisioning foreign security principles across multiple Active Directory Domain Services (AD DS) forests? Doing so will generate more workflows and requests, which results in additional load on the system.

- Will you use codeless provisioning? If so, this affects the number of expected rules entries, as well as associated requests and workflows in the system.


## See also
- [Topology considerations for deploying MIM](topology-considerations.md)
- The downloadable [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) goes into more detail about a test build and performance testing results. 
