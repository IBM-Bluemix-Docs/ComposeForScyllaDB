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

# 连接配置
{: #connection-configuration}

{{site.data.keyword.composeForScyllaDB_full}} 数据库连接通过 3 个 HAProxy 门户网站进行管理。每个门户网站有 64 MB 内存。

客户机端故障转移由应用程序设计人员负责。大多数 Scylla 驱动程序都是集群感知型，即您可以提供一个连接字符串、全部三个连接字符串、连接映射或上述各选项的某种组合，以自动处理多个连接字符串和正常故障转移。除非使用可完全处理故障转移错误的驱动程序，否则您应该：

* 使用在其连接配置中接受多个端点的驱动程序。
* 确保应用程序的数据库读写调用针对错误做出正确反应。选项包括：
  + 记录错误并重新连接。
  + 重新连接并重试导致错误的操作。
  + 将失败操作排队并安排重新连接。

## 传输中加密

所有 {{site.data.keyword.composeForScyllaDB}} HAProxy 门户网站都已启用 TLS/SSL，并且支持 TLS V1.2。用于服务的证书是 Let's Encrypt 证书。特定驱动程序和命令行界面 cqlsh 需要证书链的副本才能连接到服务；有关更多信息，请参阅[使用证书](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)。

## 连接限制

数据库连接的连接限制为每个门户网站 1000 个连接。超过连接限制将使服务无响应。可以通过驱动程序的连接池功能来管理应用程序的连接。

## 代理超时

HAProxy 门户网站可管理服务器与客户机之间连接的生命周期。我们在此详细描述的信息供驱动程序作者以及对 {{site.data.keyword.composeForScyllaDB}} 数据库连接低级别活动有兴趣的其他人员作为参考。

设置|值|适用情况
----------|-----------|-----------
client|5 分钟|客户机应该确认或发送数据的时间。
connect|4 秒|在代理与服务器之间建立连接的时间。
server|5 分钟|服务器应该确认或发送数据的时间。
tunnel|1 天|客户机与服务器之间建立双向连接，并且连接在两个方向均保持不活动状态的时间。
client-fin|1 小时|服务器应该确认或发送数据，但一个方向的连接已关闭的时间。
check|5 秒|作为辅助超时，是指检查代理与服务器之间是否建立连接的时间。

{: caption="表 1. Scylla HAProxy 超时" caption-side="top"}
