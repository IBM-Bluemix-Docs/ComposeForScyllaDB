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

# 架構 
{: #architecture}

## 抄寫

{{site.data.keyword.composeForScyllaDB_full}} 服務包含具有三個資料抄本的叢集。所有抄本都同等重要；沒有主節點或主要抄本，且每個節點都知道其他節點的狀態。您選取部署服務的地區，三個資料抄本會分散在地區的可用性區域，然後您的資料會在全部三個抄本之間複製。萬一某一個資料成員變成無法聯繫，您的叢集將繼續正常運作。

## 磁碟加密

所有 {{site.data.keyword.composeForScyllaDB}} 服務都有靜態加密。您的資料位於已啟用磁區層次加密的伺服器上。 

由於 {{site.data.keyword.composeForScyllaDB}} 是一項受管理的服務，因此 {{site.data.keyword.cloud_notm}} Compose 操作員能夠看到資料。除了磁碟加密外，您也應該先將個人資訊加密，再將它儲存在資料庫中，或使用延伸或特性對資料庫本身啟用加密。這些方法固然可能影響可用性或效能，確定個人資訊受到加密保護仍是個很好的作法。

## 自動調整

資源會根據 Scylla 資料庫的磁碟儲存空間總用量而自動調整。記憶體是以 1:10 的佈建磁碟比例為基礎。這些資源組合成為單位。

自動調整的設計是要回應資料庫的中短期趨勢。每小時都會檢查您的服務。如果它的磁碟空間不足，便會將更多的單位配置給該部署。 

自動調整對於磁碟/記憶體用量收縮的部署，不會將其縮減。針對您的資料庫佈建的資源會保留以供您未來需要，或是直到您手動縮減部署為止。
{: .tip}


