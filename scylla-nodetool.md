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

# Using nodetool
{: #scylla-nodetool}

To use nodetool to administer your {{site.data.keyword.composeForScyllaDB_full}} service, first you need to add an SSH key, then open a tunnel by using the SSH command in the _Socks_ tab of the connection strings.

When the tunnel is open, you can use `nodetool` commands, by using the format shown in the _Nodetool_ tab of the connection strings. The example command that is provided returns the status of the Scylla cluster.

## Available nodetool Commands

Nodetool has a wide range of commands, but their use is limited to avoid situations that might damage the automated maintenance of the cluster. The available commands are shown in the table.

Command|Function
----------|-----------
`cfhistograms`|Provide statistics about a table, including number of SSTables, read/write latency, partition size, and column count.
`cfstats`|Provide in-depth diagnostics regard column family.
`cleanup`|Trigger the immediate cleanup of keys no longer belonging to a node
`compact`|Force a (major) compaction on one or more column families.
`compactionhistory`|Print history of compaction.
`compactionstats`|Print statistics on compactions.
`describecluster`|Print the name, snitch, partitioner, and schema version of a cluster.
`describering <keyspace>`|Show the token ranges information of a keyspace.
`flush`|Flush one or more column families.
`help`|Prints help.
`getendpoints <keyspace> <cfname> <key>`|Print the end points that own the key.
`gossipinfo`|Show the gossip information for the cluster.
`info`|Print node information.
`move <new token>`|Move node on the token ring to a new token.
`netstats`|Print network information on provided host (connecting node by default).
`proxyhistograms`|Print statistic histograms for network operations.
`repair`|Repair one or more column families.
`ring`|Display the token ring information.
`status`|Print cluster information.
`statusbackup`|Print status of ScyllaDB backup.
`statusbinary`|Print status of native transport (binary protocol).
`statusgossip`|Print status of gossip.
`version`|Print the DB version.
{: caption="Table 1. Available nodetool commands" caption-side="top"}


## Blocked nodetool Commands

The following commands are blocked from use by nodetool when applied to a {{site.data.keyword.composeForScyllaDB}} service:

- `clearsnapshot`
- `decommision`
- `disablebackup`
- `disablebinary`
- `disablegossip`
- `drain`
- `enablebackup`
- `enablebinary`
- `enablegossip`
- `getlogginglevels`
- `listsnapshots`
- `rebuild`
- `refresh`
- `removenode`
- `setlogginglevel`
- `settraceprobability`
- `snapshot`
- `stop`
