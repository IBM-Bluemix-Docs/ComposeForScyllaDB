---

copyright:
  years: 2016,2018
lastupdated: "2018-01-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 定價
{: #pricing}

## 基本配置
{{site.data.keyword.composeForScyllaDB_full}} 服務開始於 3 個成員節點，每一個都有 5GB 的儲存空間及 512MB 的記憶體，這等於 5 個資源單位。服務_包括_ 抄寫及高可用性，因此每個單位及單價_包括_ 全部三個節點中資源的成本。

基本配置也包括 3 個 HAProxy 入口網站，以提供叢集的連線。每個 HAProxy 入口網站都有 64MB，並支援鑑別、HTTPS 及 IP 白名單。

### 成本
基本服務配置具有固定價格。請參閱 {{site.data.keyword.cloud_notm}} 上的型錄磚，以取得當地貨幣的基本定價。例如，以美元表示的基本價為 $90/月。

## 擴充選項
如果您的服務需要額外的儲存空間或記憶體，則可以使用磁碟儲存空間與記憶體單元的 10:1 比例來增加配置的資源。增加配置給部署的磁碟也會增加配置的 RAM。{{site.data.keyword.composeForScyllaDB}} 單位由 1GB 的儲存空間與 102MB 的記憶體所組成，而且每個單位及單價_包括_ 在全部三個叢集節點上增加資源的成本。

### 成本
每個額外單位（1GB 儲存空間/102MB 記憶體）都有單價，其會以當地貨幣列在服務的 {{site.data.keyword.cloud_notm}} 型錄磚上。以美元表示時，每個額外單位的成本為 $18。當您的所有 {{site.data.keyword.composeForScyllaDB}} 服務的大小_總計_ 增加時，單價就會降低，如下面分級定價表所示。

### 分級定價
單位數|單價
----------|-----------
5 - 9 個單位|基本單價 -- $18.00 美元/單位
10 - 24 個單位|折價 10% -- $16.20 美元/單位
25 - 49 個單位|折價 20% -- $14.40 美元/單位
50 - 99 個單位|折價 30% -- $12.60 美元/單位
100 - 499 個單位|折價 40% --$10.80 美元/單位
500 - 999 個單位|折價 50% -- $9.00 美元/單位
1,000 - 4,999 個單位|折價 60% -- $7.20 美元/單位
5,000 個以上單位|折價 70% -- $5.40 美元/單位
{: caption="表 1. {{site.data.keyword.composeForScyllaDB}} 分級定價" caption-side="top"}
