---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}} アプリケーションの接続

{{site.data.keyword.cloud}} アプリケーションをサービスに接続するには、サービスがプロビジョンされた時に作成されたサービス資格情報を使用します。

## 「Hello World」サンプル・アプリケーションを使用した接続

[compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) サンプル・アプリケーションでは、用意されている資格情報に基づいて Node.js によって {{site.data.keyword.composeForScyllaDB}} サービスに接続する方法を試すことができます。 このアプリケーションはデータベースを作成し、それに対して読み取りと書き込みを行います。

サンプル・アプリケーションをダウンロードし、README ファイル内の指示に従ってください。 その後、{{site.data.keyword.cloud_notm}} のアプリケーション詳細ページで**「アプリの表示」**をクリックして、*examples* 表の内容を表示してください。

## 使用可能な資格情報

フィールド名|説明
----------|-----------
`db_type`|サービスによって提供されるデータベースのタイプ。この場合は、`scylla`。
`uri_cli_1`|データベース・インスタンスに接続する 2 番目の `cqlsh` シェル・コマンド・ライン。
`maps`|さまざまなドライバーが内部 IP アドレスを ScyllaDB データベースの外部 DNS 名に関連付けるために必要な情報を提供する ScyllaDB 接続マップ。
`name`|データベース・デプロイメント名。
`uri_cli`|データベース・インスタンスに接続する `cqlsh` シェル・コマンド・ライン。
`uri_direct_2`|サービスに接続するために使用できる 3 番目の URI。 形式は、`uri` と同じです。
`uri_direct_1`|サービスに接続するために使用できる 2 番目の URI。 形式は、`uri` と同じです。
`ca_certificate_base64` `(オプション)`|base64 でエンコードされている、アプリケーションが適切なサーバーに接続されていることを確認するために使用する自己署名証明書。 自己署名証明書は、Let's Encrypt 証明書があるサービスには存在しません。 サンプル・アプリケーションで示されているように、鍵をデコードした後に使用する必要があります。
`deployment_id`|Compose 内で作成された、サービスの内部 ID。
`uri_cli_2`|データベース・インスタンスに接続する 3 番目の `cqlsh` シェル・コマンド・ライン。
`uri`|サービスに接続するときに使用する URI。スキーマ (`scylla:`)、パスワード、サーバーのホスト名、接続先のポート番号、データベース名が含まれます。
{: caption="表 1. Compose for ScyllaDB の資格情報" caption-side="top"}

**注:** 3 つの `haproxy` ポータルは、Scylla サービスへのアクセスを提供します。 接続するために `uri`、`uri_direct_1`、および `uri_direct_2` を使用できます。 アプリケーションで、接続障害の対応管理のために `uri`、`uri_direct_1`、および `uri_direct_2` を切り替えます。
