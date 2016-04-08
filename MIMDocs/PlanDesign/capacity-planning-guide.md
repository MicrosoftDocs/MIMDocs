---
# required metadata

title: Capacity planning guide | Microsoft Identity Manager
description: Use this guide to understand the variables that should be considered before deploying MIM 2016, including load levels and policy decisions.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
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

This guide was produced to assist in customer planning, but it should not be used alone to determine the appropriate servers, hardware, or topologies that are required for a Microsoft Identity Manager (MIM) deployment. Organizations are encouraged and expected to configure their own test environments to more accurately estimate capacity and performance. Microsoft cannot guarantee that organizations will experience the same capacity or performance characteristics, even if the MIM 2016 components are deployed and configured identically to the components that are described in this guide.

To prepare properly for your MIM deployment, simulate your anticipated production environment in a lab setting and test it. You may decide to test different topologies running on different types of hardware, and then run different scale and load scenario tests, which can help you better estimate what may happen when you deploy MIM 2016 in your environment.


## Overview
There are a number of variables that can affect the overall capacity and performance of your Microsoft Identity Manager deployment. The ways in which you physically deploy the MIM components (topology), as well as the hardware on which those components run, are important factors in determining the performance and capacity that you can expect from your MIM deployment. The number and complexity of the MIM policy configuration objects may be less obvious, but they are still significant factors to consider when planning for capacity. Finally, the expected scale of the deployment, as well as the load that is expected to be placed on it, are typically more obvious factors that affect performance and capacity.

The main factors that affect the capacity and performance that can be expected from a MIM 2016 deployment are discussed in the following table.

| Design Factor | Considerations |
| ------------- | -------------- |
| Topology | The distribution of the MIM services among computers on the network. For example, will the MIM 2016 Synchronization Service be hosted on the same computer that hosts its database? |
| Hardware | The physical hardware and any virtualized hardware specifications that you are running for each MIM component. This includes CPU, memory, network adapter, and hard drive configuration. |
| MIM policy configuration objects | The number and type of MIM policy configuration objects, which includes sets, Management Policy Rules (MPRs), and workflows. For example, how many workflows are triggered for operations? How many set definitions exist and what is the relative complexity of each? |
| Scale | The number of users, groups, calculated groups, and custom object types, such as computers to be managed by MIM 2016. Also, consider the complexity of dynamic groups, and be sure to factor in group nesting. |
| Load | Frequency of anticipated use. For example, the number of times that you expect new groups or users to be created, passwords to be reset, or the portal to be visited in a given time period. Note that the load may vary during the course of an hour, day, week, or year. Depending on the component, you may have to design for peak load or average load.


## Hosting Microsoft Identity Manager components
Microsoft Identity Manager has many different components. In many cases, these components are not located on the same computer. When you think of MIM from a capacity planning perspective, the components and the physical computers (and, possibly, virtual machines) on which they are hosted are key considerations. Many potential factors can affect the performance of the MIM environment, for example, the physical disk configuration for the computer running the MIM 2016 Service SQL Database. The number of spindles that make up the disk configuration or the distribution of log and data files can affect the performance of the system significantly. Also, consider the external factors in your configuration. If you are using a SAN as the MIM 2016 Service database configuration, what other applications are sharing the SAN? How do these applications affect database performance as they compete for the shared disk resources on the SAN? When multiple applications compete for the same disk resources, bottlenecks and irregular disk performance might be the result.


## Users and groups
The number of users and groups in your environment is a typical consideration when you think about the scale of a deployment. However, there are several other related considerations that you should also factor into planning. These other considerations include the following:

- Can users create groups? If so, you should consider estimating how users creating new groups will affect the growth of groups in your environment.

- Will dynamic groups be deployed?
  - What types of dynamic groups will be deployed?
  - How many dynamic groups are likely to be deployed?


## Expected load levels
You should also consider the type of load that will be placed on the MIM components. Some relevant questions to consider include the following:

- How often do you expect a request to join or leave a group?

- How often do you expect a user to create a static or dynamic group?

- Can you obtain this information from existing applications in your environment?

- How much load do you expect from non-user-driven operations, such as the synchronization of changes from external systems? Ensure that you account for load that is generated by synchronization operations of identity information with external systems.

- What kind of scenarios do you plan to deploy? Different scenarios will contribute to different load patterns. For example, client computers that have the MIM 2016 client installed periodically validate if registration is required at sign-in, which increases your system load.

- Do you expect large variations in load levels, from normal to peak load? For example, do you expect large numbers of password resets after holiday periods? Make sure that you work your system maintenance and synchronization schedules outside the anticipated usage peaks. When you think of capacity planning, ensure that you factor in peak load periods.


## Policy configuration objects

Microsoft Identity Manager policy configuration objects represent the business logic for the MIM deployment. This is one area in which each MIM implementation will probably be unique because policy configuration is specific to the needs of each deployment. MIM policy configuration objects include the MPRs, sets, workflows, and synchronization rules for a particular deployment. Key performance considerations related to MIM policy configuration objects include the following:

- **Sets** Every operation in the system must be evaluated against existing set memberships and updates that cause changes in set membership. For example, a simple change, such as change to the building number of an individual’s office, may not have a huge impact. However, other changes might have a cascading impact, such as the change of a manager, which may affect multiple objects at multiple levels.

- **Management Policy Rules** MPRs are used for controlling access control rules and triggering workflows. As you create MPRs, you may find that there is a need to increase the number of sets so that you can capture various object transition states. These additional sets may trigger additional workflows, with each workflow mapping to unique requests in the system. This then becomes another item to include when you plan capacity.

While working on MIM policy configuration objects, you should also consider the following:

- Will you be provisioning foreign security principles across multiple Active Directory Domain Services (AD DS) forests? Doing so will generate more workflows and requests, which results in additional load on the system.

- Will you use codeless provisioning? If so, this affects the number of expected rules entries, as well as associated requests and workflows in the system.


## See also
- The downloadable [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) goes into more detail about a test build and performance testing results.
