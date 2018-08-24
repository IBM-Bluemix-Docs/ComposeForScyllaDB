---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# 连接 {{site.data.keyword.cloud_notm}} 应用程序

要将 {{site.data.keyword.cloud}} 应用程序连接到服务，请使用供应服务时所创建的服务凭证。

## 使用“Hello World”样本应用程序进行连接

[compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) 样本应用程序演示如何使用 Node.js，通过提供的凭证连接到 {{site.data.keyword.composeForScyllaDB}} 服务。应用程序创建、读取和写入数据库。

下载样本应用程序，并遵循自述文件中的指示信息。然后，在 {{site.data.keyword.cloud_notm}} 中的应用程序详细信息页面中，单击**查看应用程序**，以查看*示例*表的内容。

## 可用凭证

字段名称|描述
----------|-----------
`db_type`|服务所提供的数据库类型；在本例中为 `scylla`。
`uri_cli_1`|连接到数据库实例的第二个 `cqlsh` shell 命令行。
`maps`|ScyllaDB 连接映射，提供各种驱动程序将内部 IP 地址与 ScyllaDB 数据库的外部 DNS 名称相关联所需的信息。
`name`|数据库部署名称。
`uri_cli`|连接到数据库实例的 `cqlsh` shell 命令行。
`uri_direct_2`|可用于连接到服务的第三个 URI。格式与 `uri` 相同。
`uri_direct_1`|可用于连接到服务的第二个 URI。格式与 `uri` 相同。
`ca_certificate_base64``（可选）`|Base64 编码的自签名证书，用于确认应用程序连接到的是相应的服务器。在具有 Let's Encrypt 证书的服务上不提供该自签名证书。您需要先将密钥解码，才能加以使用，如样本应用程序中所示。
`deployment_id`|在 Compose 内创建的服务的内部标识。
`uri_cli_2`|连接到数据库实例的第三个 `cqlsh` shell 命令行。
`uri`|连接到服务时使用的 URI，包括模式 (`scylla:`)、密码、服务器的主机名、要连接到的端口号和数据库名称。
{: caption="表 1. Compose for ScyllaDB 凭证" caption-side="top"}

**注：**三个 `haproxy` 门户网站提供对 Scylla 服务的访问权。`uri`、`uri_direct_1` 和 `uri_direct_2` 都可用于连接。在您的应用程序中，在 `uri`、`uri_direct_1` 和 `uri_direct_2` 之间进行切换，以管理对连接失败的响应。
