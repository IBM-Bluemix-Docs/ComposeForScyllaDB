---

copyright:
  years: 2017,2018
lastupdated: "2017-12-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 nodetool
{: #using-nodetool}

要使用 nodetool 来管理 {{site.data.keyword.composeForScyllaDB_full}} 服务，必须先添加 SSH 密钥，然后在连接字符串的 _Socks_ 选项卡中使用 SSH 命令打开隧道。

隧道打开后，可以使用 `nodetool` 命令，格式如连接字符串的 _Nodetool_ 选项卡中所示。提供的命令会返回 Scylla 集群的状态。

## 可用的 nodetool 命令

Nodetool 有各种各样的命令，但命令的使用受到限制，以避免可能会损害集群自动维护的情况。可用命令如下表所示。

命令|功能
----------|-----------
`cfhistograms`|提供有关表的统计信息，包括 SSTable 的数量、读/写等待时间、分区大小和列计数。
`cfstats`|提供关于列系列的深入诊断。
`cleanup`|触发立即清除不再属于节点的密钥。
`compact`|对一个或多个列系列执行强制（高度）压缩。
`compactionhistory`|打印压缩历史记录。
`compactionstats`|打印有关压缩的统计信息。
`describecluster`|打印集群的名称、snitch、分区程序和模式版本。
`describering <keyspace>`|显示密钥空间的令牌范围信息。
`flush`|清空一个或多个列系列。
`help`|打印帮助。
`getendpoints <keyspace> <cfname> <key>`|打印拥有密钥的端点。
`gossipinfo`|显示集群的闲话信息。
`info`|打印节点信息。
`move <new token>`|在令牌环上将节点移至新令牌。
`netstats`|打印有关所提供主机（缺省情况下为连接节点）的网络信息。
`proxyhistograms`|打印网络操作的统计直方图。
`repair`|修复一个或多个列系列。
`ring`|显示令牌环信息。
`status`|打印集群信息。
`statusbackup`|打印 ScyllaDB 备份的状态。
`statusbinary`|打印本机传输的状态（二进制协议）。
`statusgossip`|打印闲话的状态。
`version`|打印数据库版本。
{: caption="表 1. 可用的 nodetool 命令" caption-side="top"}


## 阻止使用的 nodetool 命令

nodetool 应用于 {{site.data.keyword.composeForScyllaDB}} 服务时，将阻止 nodetool 使用以下命令：

- clearsnapshot
- decommision
- disablebackup
- disablebinary
- disablegossip
- drain
- enablebackup
- enablebinary
- enablegossip
- getlogginglevels
- listsnapshots
- rebuild
- refresh
- removenode
- setlogginglevel
- settraceprobability
- snapshot
- stop
