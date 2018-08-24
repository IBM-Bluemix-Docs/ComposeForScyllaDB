---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 体系结构 
{: #architecture}

## 复制

{{site.data.keyword.composeForScyllaDB_full}} 服务包含一个具有三个数据副本的集群。所有副本都同等重要；没有主副本，并且每个节点都知道其他节点的状态。您可选择部署了该服务的区域，三个数据副本会分布在该区域的可用性专区中，然后数据会在所有三个副本之间进行复制。如果一个数据成员变为不可访问，集群仍会继续正常运行。

## 磁盘加密

所有 {{site.data.keyword.composeForScyllaDB}} 服务都具有静态加密。数据位于启用了卷级别加密的服务器上。 

由于 {{site.data.keyword.composeForScyllaDB}} 是受管服务，因此 {{site.data.keyword.cloud_notm}} Compose 操作员可以查看数据。除了磁盘加密外，您还应该在数据库中存储个人信息之前，对这些信息加密，或者使用扩展或功能启用对数据库本身的加密。虽然这些方法可能会影响可用性或性能，但确保个人信息受到加密保护是比较好的做法。

## 自动扩展

资源将根据 Scylla 数据库的磁盘存储总使用量自动进行扩展。内存基于按 1:10 比率供应的磁盘。这些资源会捆绑为单元。

自动扩展旨在响应数据库的中短期趋势。系统每小时会检查一次服务。如果服务的磁盘空间不足，那么会向部署分配更多单元。 

自动扩展不会向下扩展部署，即减少磁盘/内存使用量。向数据库供应的资源将保留以满足您的未来需求，或者直到您手动向下扩展部署为止。
{: .tip}


