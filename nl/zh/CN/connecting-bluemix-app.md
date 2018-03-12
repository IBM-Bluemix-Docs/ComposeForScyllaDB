---

copyright:
  years: 2016,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 连接 {{site.data.keyword.cloud_notm}} 应用程序

要将 {{site.data.keyword.cloud}} 应用程序连接到服务，请使用供应服务时所创建的服务凭证。样本应用程序演示如何使用 Node.js，通过提供的凭证连接到 {{site.data.keyword.composeForScyllaDB_full}} 服务，以及如何创建数据库以及如何对数据库进行读取和写入操作。

## 使用“Hello World”样本应用程序进行连接

[compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) 样本应用程序演示如何使用 Node.js，通过提供的凭证连接到 {{site.data.keyword.composeForScyllaDB}} 服务。应用程序创建、读取和写入数据库

下载样本应用程序，并遵循自述文件中的指示信息。然后，在 {{site.data.keyword.cloud_notm}} 中的应用程序详细信息页面中，单击**查看应用程序**，以查看*示例*表的内容。

## 可用凭证

字段名称|描述
----------|-----------
`db_type`|服务所提供的数据库类型；在本例中为 `scylla`。
`uri_cli_1`|连接到数据库实例的替代 `cqlsh` shell 命令行。
`maps`|ScyllaDB 连接映射，提供各种驱动程序将内部 IP 地址与 ScyllaDB 数据库的外部 DNS 名称相关联所需的信息。
`name`|数据库部署名称。
`uri_cli`|连接到数据库实例的 `cqlsh` shell 命令行。
`uri_direct_2`|可用于连接到服务的替代 URI。格式与 `uri` 一样。
`uri_direct_1`|可用于连接到服务的替代 URI。格式与 `uri` 一样。
`ca_certificate_base64`|自签名证书，用于确认应用程序连接到相应的服务器。此证书是 Base64 编码。
`deployment_id`|在 Compose 内创建的服务的内部标识。
`uri_cli_2`|连接到数据库实例的替代 `cqlsh` shell 命令行。
`uri`|连接到服务时使用的 URI，包括模式 (`scylla:`)、密码、服务器的主机名、要连接到的端口号和数据库名称。
{: caption="表 1. Compose for ScyllaDB 凭证" caption-side="top"}
