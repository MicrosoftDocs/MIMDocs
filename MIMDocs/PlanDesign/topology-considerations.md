---
# required metadata

title: Topology considerations for deploying MIM | Microsoft Identity Manager
description: Understand the MIM 2016 components, and get suggestions for how to deploy them in your environment.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---


# Topology considerations
You can deploy Microsoft Identity Manager (MIM) components on the same server or among multiple servers in multiple configurations. The topology that you select for your deployment affects the performance that you can achieve from MIM. This article introduces multiple deployment topologies that you may consider implementing.

## MIM components
When designing your deployment topology, it's important to know what each component does and how they all interact.

- **MIM Portal** - an interface for password resets, group management, and administrative operations.
    -
- **MIM Service** - a web service that implements MIM 2016 identity management functionality.
- **MIM Synchronization Service** - Synchronizes data with other identity systems.
- **Microsoft SQL Server** - MIM Service and MIM Sync Service both store their data in SQL databases.

The following table shows the options for hosting each of the MIM components. They can be hosted on the same computer, or distributed among multiple servers and clusters.

| | MIM Portal | MIM Service | MIM Sync Service | SQL Server |
| --- | --- | --- | --- | --- |
| Same computer | Yes | Yes | Yes | Yes |
| Separate server | Yes | Yes | Yes | Yes |
| Network Load Balancing cluster | Yes | Yes | | |
| Server cluster | | | | Yes |


## Multitier topology
The multitier topology is the most commonly used topology. It offers the greatest flexibility. The MIM Portal, MIM Service, and databases are separated into tiers and deployed on multiple computers. This topology adds flexibility in scaling the different MIM components. For example, you can scale the MIM Portal horizontally by adding additional servers in a Network Load Balancing (NLB) cluster. Similarly, you can scale the MIM service by using an NLB cluster and by increasing the number of computers (nodes) in the cluster as needed.

In the multitier topology, a dedicated computer to host each SQL database (one for the MIM Service and another for the MIM Synchronization Service) is allocated. The scalability of the performance of the computers that host the SQL databases can be increased by adding or upgrading hardware, for example, by upgrading the CPUs, adding additional CPUs, increasing random access memory (RAM) or upgrading the RAM, or upgrading the hard drive configurations to increase read and write access and decrease latency.

![MIM multitier topology diagram](media/MIM-topo-multitier.png)

In this configuration, the MIM Synchronization Service and its database are hosted on the same computer. However, you should be able to achieve similar performance if there is a one-gigabit dedicated network connection between the MIM Synchronization Service and its database when they are hosted on separate computers.


## Multitier topology with multiple MIM services
Synchronizing data with external systems can take a long time and add a considerable load to the system during that period. If the synchronization configuration results in triggering policies with workflows, these policies contend for resources with end-user workflows. Such issues can be pronounced with authentication workflows, such as password resets, which are done in real time with an end user waiting for the process to complete. By providing one instance of the MIM Service for end user operations and a separate portal for administrative data synchronization, you can provide better responsiveness for end-user operations.

![Multiple MIM multitier topology diagram](media/MIM-topo-multitier-multiservice.png)

As with the standard multitier topology, you can increase MIM Portal performance by using an NLB cluster and by increasing the number of nodes in the cluster as needed.

The computers running SQL Server that host the MIM Synchronization Service and the MIM Service database will dramatically influence the overall performance of your MIM deployment. Therefore, follow the recommendations in SQL Server documentation for optimizing database performance. See the following documents for more information:

## See also
- The downloadable [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) goes into more detail about a test build and performance testing results.
