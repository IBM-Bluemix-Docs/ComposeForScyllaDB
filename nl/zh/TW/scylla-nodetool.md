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

# 使用 Nodetool
{: #using-nodetool}

若要使用 nodetool 來管理 {{site.data.keyword.composeForScyllaDB_full}} 服務，您必須先新增 SSH 金鑰，然後在連線字串的 _Socks_ 標籤中使用 SSH 指令來開啟通道。

當通道開啟時，您可以使用 `nodetool` 指令與連線字串的 _Nodetool_ 標籤中顯示的格式搭配。提供的指令會傳回 Scylla 叢集的狀態。

## 可用的 Nodetool 指令

Nodetool 具有廣泛的指令，但它們的使用受到限制，以避免發生可能損壞叢集自動化維護的狀況。表格中顯示可用的指令。

指令|功能
----------|-----------
`cfhistograms`|提供有關表格的統計資料，包括 SSTable 數目、讀寫延遲、分割區大小，以及直欄計數。
`cfstats`|提供針對直欄系列的深入診斷。
`cleanup`|觸發不再屬於節點之金鑰的立即清除
`compact`|對一個以上的直欄系列強制執行（主要）壓縮。
`compactionhistory`|列印壓縮歷程。
`compactionstats`|列印壓縮的統計資料。
`describecluster`|列印叢集的名稱、告發者、分割程式及綱目版本。
`describering <keyspace>`|顯示金鑰空間的記號範圍資訊。
`flush`|清除一個以上的直欄系列。
`help`|列印說明。
`getendpoints <keyspace> <cfname> <key>`|列印擁有金鑰的端點。
`gossipinfo`|顯示叢集的小道消息資訊。
`info`|列印節點資訊。
`move <new token>`|將記號環上的節點移至新的記號。
`netstats`|列印有關所提供主機（依預設連接節點）的網路資訊。
`proxyhistograms`|列印網路作業的統計資料直方圖。
`repair`|修復一個以上的直欄系列。
`ring`|顯示記號環資訊。
`status`|列印叢集資訊。
`statusbackup`|列印 ScyllaDB 備份的狀態。
`statusbinary`|列印原生傳輸的狀態（二進位通訊協定）。
`statusgossip`|列印小道消息的狀態。
`version`|列印 DB 版本。
{: caption="表 1. 可用的 Nodetool 指令" caption-side="top"}


## 封鎖的 Nodetool 指令

套用至 {{site.data.keyword.composeForScyllaDB}} 服務時，Nodetool 禁止使用下列指令：

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
