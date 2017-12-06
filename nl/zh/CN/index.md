---

copyright:
  years: 2016,2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 开始使用 Compose for ScyllaDB
{: #getting-started-with-compose-for-scylladb}

ScyllaDB 是 Cassandra 宽列分布式数据库的内部替代产品。ScyllaDB 是用 C++ 而不是 Cassandra 的 Java 编写的，目的是更好地利用资源，使基准性能提高到原先的 10 倍。除了保留与 Cassandra 工具和数据文件的兼容性外，ScyllaDB 还添加了自调整功能。通过 {{site.data.keyword.composeForScyllaDB_full}}，ScyllaDB 的功能得到扩展，不仅可以为您管理 ScyllaDB ，还提供了易于使用、自动扩展的部署系统，能交付高可用性、冗余性和自动备份功能。
{:shortdesc}

**注：**{{site.data.keyword.composeForScyllaDB_full}} 不会授予对 Compose UI 的访问权。请参阅 [Compose on {{site.data.keyword.cloud}} 支持](https://help.compose.com/docs/bluemix-compose-support)，以获取更多详细信息。

## 创建 Compose for ScyllaDB 服务实例

[创建 {{site.data.keyword.composeForScyllaDB}} 实例](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/)。

当您创建服务实例时，请选择服务的名称和凭证名称。保持服务处于未绑定状态；您可以稍后使用供应服务时提供的凭证，将应用程序连接到您的服务。“可用凭证”一节列出了各种凭证值。

供应 {{site.data.keyword.composeForScyllaDB}} 实例时，可以选择*标准*或*企业*套餐。使用*企业*套餐，您可以将 {{site.data.keyword.composeForScyllaDB}} 实例供应到可用的 {{site.data.keyword.composeEnterprise}} 集群中。{{site.data.keyword.composeEnterprise}} 提供企业合规性所需的安全性和隔离，并使用专用网络来确保已部署的数据库的性能。有关更多详细信息，请参阅 [Compose Enterprise 文档](../ComposeEnterprise/index.html)。

## 管理 Compose for ScyllaDB

您可以从服务仪表板管理服务。您可以在此找到有关 {{site.data.keyword.cloud_notm}} Compose 数据库以及如何连接到该数据库的信息。您还可以：

- 管理备份
- 为服务分配更多资源 
- 使用白名单来限制对数据库的访问。 

有关更多信息，请参阅[设置](./dashboard-settings.html)。

## 连接到 Compose for ScyllaDB

您可以使用随服务一起创建的凭证，或者使用服务仪表板*概述*选项卡中提供的连接字符串和命令行，来连接到服务。

## 将 {{site.data.keyword.cloud_notm}} 应用程序连接到 Compose for ScyllaDB

要将 {{site.data.keyword.cloud_notm}} 应用程序连接到服务，请使用随服务一起创建的凭证。您可以在[连接 {{site.data.keyword.cloud_notm}} 应用程序](./connecting-bluemix-app.html)中查找有关如何将 {{site.data.keyword.cloud_notm}} 应用程序连接到 {{site.data.keyword.composeForScyllaDB}} 服务的信息。

## 从外部 {{site.data.keyword.cloud_notm}} 连接到 Compose for ScyllaDB

如果要从外部 {{site.data.keyword.cloud_notm}} 连接到 {{site.data.keyword.composeForScyllaDB}}，可以使用提供的连接字符串或命令行。您可以在[连接外部应用程序](./connecting-external.html)中找到有关如何连接的信息。
