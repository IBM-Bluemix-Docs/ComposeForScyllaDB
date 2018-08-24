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

# {{site.data.keyword.composeForScyllaDB}} の概要
{: #about-compose-for-scylladb}

ScyllaDB は、Cassandra ワイドカラム分散データベースをインプレースで置き換えたものです。 ScyllaDB は、リソースをより有効に使用してベンチマークでのパフォーマンスが 10 倍に向上するように、Cassandra の Java ではなく C++ で記述されています。 Cassandra のツールやデータ・ファイルとの互換性を保持することに加えて、
ScyllaDB ではセルフチューニングの機能が追加されます。 {{site.data.keyword.composeForScyllaDB_full}} は、ScyllaDB をユーザーの代わりに管理して、その機能を拡張することにより、
高可用性、冗長度、自動バックアップを備えた、使いやすい自動スケーリング・デプロイメント・システムを提供します。
{:shortdesc}

## {{site.data.keyword.composeForScyllaDB}} サービス・インスタンスの作成

{{site.data.keyword.composeForScyllaDB}} サービスは、{{site.data.keyword.cloud_notm}} カタログの [{{site.data.keyword.composeForScyllaDB}} ページ](https://console.{DomainName}/catalog/services/compose-for-scylladb/)から作成できます。

サービス名、およびサービスをプロビジョンする地域、組織、スペースを選択します。 **「データベースのバージョンの選択 (Select a database version)」**フィールドを使用して、データベースの使用できるバージョンを選択できます。

{{site.data.keyword.composeForScyllaDB}} インスタンスをプロビジョンするときには、*標準*プランか*エンタープライズ*・プランを選択できます。 *エンタープライズ*・プランでは、{{site.data.keyword.composeForScyllaDB}} インスタンスを利用可能な {{site.data.keyword.composeEnterprise}} クラスターにプロビジョンできます。 {{site.data.keyword.composeEnterprise}} は、企業コンプライアンスで要求されるセキュリティーと分離を提供し、専用ネットワーキングを使用してデプロイ済みデータベースのパフォーマンスを確保します。 詳しくは、[{{site.data.keyword.composeEnterprise}} 文書](/docs/services/ComposeEnterprise/index.html)を参照してください。

## {{site.data.keyword.composeForScyllaDB}} の管理

サービス・ダッシュボードからサービスを管理できます。 ダッシュボードには {{site.data.keyword.cloud_notm}} Compose データベースとそれへの接続方法に関する情報が示されます。 また、以下の操作を行うこともできます。

- バックアップを管理する
- サービスに割り振るリソースを増やす 
- ホワイトリストを使用してデータベースへのアクセスを制限する。 

詳しくは、[設定](./dashboard-settings.html)を参照してください。

{{site.data.keyword.composeForScyllaDB}} は、Cloud Foundry の役割を利用して、サービスへのアクセスを管理します。 開発者役割を持つユーザーのみが、サービス・ダッシュボードを表示または使用できます。 Cloud Foundry の役割について詳しくは、『[Cloud Foundry アクセス権限](https://console.{DomainName}/docs/iam/cfaccess.html#cfaccess)』および 『[Cloud Foundry のアクセス権限の管理](https://console.{DomainName}/docs/iam/mngcf.html#mngcf)』のページを参照してください。
{: .tip}

## {{site.data.keyword.composeForScyllaDB}} への接続

サービスに接続するには、サービスと一緒に作成された資格情報を使用するか、サービス・ダッシュボードの*「概要」*タブに表示される接続ストリングとコマンド・ラインを使用します。

## {{site.data.keyword.composeForScyllaDB}} への {{site.data.keyword.cloud_notm}} アプリケーションの接続

{{site.data.keyword.cloud_notm}} アプリケーションをサービスに接続するには、サービスと一緒に作成された資格情報を使用します。 {{site.data.keyword.cloud_notm}} アプリケーションを {{site.data.keyword.composeForScyllaDB}} サービスに接続する方法については、[{{site.data.keyword.cloud_notm}} アプリケーションの接続](./connecting-bluemix-app.html)を参照してください。

## {{site.data.keyword.cloud_notm}} 外からの {{site.data.keyword.composeForScyllaDB}} への接続

{{site.data.keyword.composeForScyllaDB}} に {{site.data.keyword.cloud_notm}} の外部から接続するには、提供された接続ストリングまたはコマンド・ラインを使用します。 接続方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。
