---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"

keywords: scylla, compose

subcollection: ComposeForScyllaDB

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Architecture 
{: #architecture}

## Replication

An {{site.data.keyword.composeForScyllaDB_full}} service contains a cluster with three data replicas. All replicas are equally important; there is no primary or master replica, and each node knows the state the other nodes. You select a region where the service is deployed, the three data replicas are spread over the region's availability zones, and your data is copied across all three. If one data member becomes unreachable, your cluster continues to operate normally.

## Disk Encryption

All {{site.data.keyword.composeForScyllaDB}} services have encryption at rest. Your data resides on servers that have volume-level encryption enabled. 

Because {{site.data.keyword.composeForScyllaDB}} is a managed service, {{site.data.keyword.cloud_notm}} Compose operators can see data. If you're storing personal information in your database, encrypt it before you store it, or use extensions or features to enable encryption on the database itself. While these methods can impact usability or performance, it's good practice to ensure that personal information is protected with encryption.

## Auto-scaling

Resources are scaled automatically based on the total disk storage use of the Scylla databases. Memory is based on a ratio of provisioned disk at 1:10. These resources are bundled as units.

Auto-scaling is designed to respond to the short-to-medium term trends of your database. Your service is checked every hour. If it is running short on disk space, then more units are allocated to the deployment. 

Auto-scaling does not scale down deployments where disk or memory usage is reduced. The resources provisioned to your databases remain for your future needs, or until you scale down your deployment manually.
{: tip}


