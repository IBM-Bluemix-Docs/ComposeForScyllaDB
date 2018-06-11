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

# 關於 {{site.data.keyword.composeForScyllaDB}}
{: #about-compose-for-scylladb}

ScyllaDB 是 Cassandra 寬直欄分散式資料庫的就地取代。ScyllaDB 以 C++ 撰寫，而不是 Cassandra 的 Java，它能夠更充份利用資源，在基準性能測試時可達到 10 倍的效能。除了維持與 Cassandra 工具及資料檔案的相容性以外，ScyllaDB 也新增了自行調整的功能。{{site.data.keyword.composeForScyllaDB_full}} 延伸了 ScyllaDB 的功能，因為它能替您管理、提供容易、自動擴充的部署系統，此系統能提供高可用性與備援，也能提供自動化備份。
{:shortdesc}

## 建立 {{site.data.keyword.composeForScyllaDB}} 服務實例

您可以從 {{site.data.keyword.cloud_notm}} 型錄中的 [{{site.data.keyword.composeForScyllaDB}} 頁面](https://console.{DomainName}/catalog/services/compose-for-scylladb/)建立 {{site.data.keyword.composeForScyllaDB}} 服務。

選擇服務名稱、地區、組織，以及要在其中佈建服務的空間。您可以使用**選取資料庫版本**欄位，從可用的資料庫版本中進行選擇。

當您佈建 {{site.data.keyword.composeForScyllaDB}} 實例時，可以選擇*標準* 或*企業* 方案。使用*企業* 方案，您可以將 {{site.data.keyword.composeForScyllaDB}} 實例佈建到可用的 {{site.data.keyword.composeEnterprise}} 叢集。{{site.data.keyword.composeEnterprise}} 提供企業相符性所需的安全和隔離，並使用專用網路來確保已部署之資料庫的效能。如需詳細資料，請參閱 [Compose Enterprise 文件](../ComposeEnterprise/index.html)。

## 管理 {{site.data.keyword.composeForScyllaDB}}

您可以從服務儀表板來管理服務。在這裡，您可以找到 {{site.data.keyword.cloud_notm}} Compose 資料庫的相關資訊，以及連接方式。您也可以：

- 管理備份
- 為您的服務配置更多資源 
- 使用白名單來限制對資料庫的存取權。 

如需相關資訊，請參閱[設定](./dashboard-settings.html)。

{{site.data.keyword.composeForScyllaDB}} 根據 Cloud Foundry 角色來管理對服務的存取。只有具有「開發人員」角色的使用者才能看到或使用服務儀表板。如需 Cloud Foundry 角色的相關資訊，請參閱 [Cloud Foundry 存取](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess)及[管理 Cloud Foundry 存取](https://console.bluemix.net/docs/iam/mngcf.html#mngcf)頁面。
{: .tip}

## 連接至 {{site.data.keyword.composeForScyllaDB}}

您可以使用與服務一起建立的認證，或使用服務儀表板的*概觀* 標籤中所提供的連線字串及指令行，來連接至服務。

## 將 {{site.data.keyword.cloud_notm}} 應用程式連接至 {{site.data.keyword.composeForScyllaDB}}

若要將 {{site.data.keyword.cloud_notm}} 應用程式連接至服務，請使用與服務一起建立的認證。您可以在[連接 {{site.data.keyword.cloud_notm}} 應用程式](./connecting-bluemix-app.html)中，找到如何將 {{site.data.keyword.cloud_notm}} 應用程式連接至 {{site.data.keyword.composeForScyllaDB}} 服務的資訊。

## 從 {{site.data.keyword.cloud_notm}} 之外連接至 {{site.data.keyword.composeForScyllaDB}}

如果您想要從 {{site.data.keyword.cloud_notm}} 之外連接至 {{site.data.keyword.composeForScyllaDB}}，則可以使用所提供的連線字串或指令行。您可以在[連接外部應用程式](./connecting-external.html)中，找到如何連接的相關資訊。
