---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 关于 {{site.data.keyword.composeForScyllaDB}}
{: #about-compose-for-scylladb}

ScyllaDB 是 Cassandra 宽列分布式数据库的内部替代产品。ScyllaDB 是用 C++ 而不是 Cassandra 的 Java 编写的，目的是更好地利用资源，使基准性能提高到原先的 10 倍。除了保留与 Cassandra 工具和数据文件的兼容性外，ScyllaDB 还添加了自调整功能。通过 {{site.data.keyword.composeForScyllaDB_full}}，ScyllaDB 的功能得到扩展，不仅可以为您管理 ScyllaDB，还提供了易于使用、自动扩展的部署系统，能交付高可用性、冗余性和自动备份功能。
{:shortdesc}

## 创建 {{site.data.keyword.composeForScyllaDB}} 服务实例

您可以在 {{site.data.keyword.cloud_notm}}“目录”的 [{{site.data.keyword.composeForScyllaDB}} 页面](https://console.{DomainName}/catalog/services/compose-for-scylladb/)中创建 {{site.data.keyword.composeForScyllaDB}} 服务。

选择服务名称以及要在其中供应该服务的区域、组织和空间。可以使用**选择数据库版本**字段从可用数据库版本中进行选择。

供应 {{site.data.keyword.composeForScyllaDB}} 实例时，可以选择*标准*或*企业*套餐。使用*企业*套餐，您可以将 {{site.data.keyword.composeForScyllaDB}} 实例供应到可用的 {{site.data.keyword.composeEnterprise}} 集群中。{{site.data.keyword.composeEnterprise}} 提供企业合规性所需的安全性和隔离，并使用专用网络来确保已部署的数据库的性能。有关更多详细信息，请参阅 [{{site.data.keyword.composeEnterprise}} 文档](/docs/services/ComposeEnterprise/index.html)。

## 管理 {{site.data.keyword.composeForScyllaDB}}

您可以从服务仪表板管理服务。您可以在此找到有关 {{site.data.keyword.cloud_notm}} Compose 数据库以及如何连接到该数据库的信息。您还可以：

- 管理备份
- 为服务分配更多资源 
- 使用白名单来限制对数据库的访问。 

有关更多信息，请参阅[设置](./dashboard-settings.html)。

{{site.data.keyword.composeForScyllaDB}} 依赖于 Cloud Foundry 角色来管理对服务的访问权。只有具有开发者角色的用户才能查看或使用服务仪表板。有关 Cloud Foundry 角色的更多信息，请参阅 [Cloud Foundry 访问权](https://console.{DomainName}/docs/iam/cfaccess.html#cfaccess)和[管理 Cloud Foundry 访问权](https://console.{DomainName}/docs/iam/mngcf.html#mngcf)页面。
{: .tip}

## 连接到 {{site.data.keyword.composeForScyllaDB}}

您可以使用随服务一起创建的凭证，或者使用服务仪表板*概述*选项卡中提供的连接字符串和命令行，来连接到服务。

## 将 {{site.data.keyword.cloud_notm}} 应用程序连接到 {{site.data.keyword.composeForScyllaDB}}

要将 {{site.data.keyword.cloud_notm}} 应用程序连接到服务，请使用随服务一起创建的凭证。您可以在[连接 {{site.data.keyword.cloud_notm}} 应用程序](./connecting-bluemix-app.html)中查找有关如何将 {{site.data.keyword.cloud_notm}} 应用程序连接到 {{site.data.keyword.composeForScyllaDB}} 服务的信息。

## 从外部 {{site.data.keyword.cloud_notm}} 连接到 {{site.data.keyword.composeForScyllaDB}}

如果要从外部 {{site.data.keyword.cloud_notm}} 连接到 {{site.data.keyword.composeForScyllaDB}}，可以使用提供的连接字符串或命令行。您可以在[连接外部应用程序](./connecting-external.html)中找到有关如何连接的信息。
