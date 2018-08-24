---

copyright:
  years: 2017,2018
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# バックアップ
{: #backups}

サービス・ダッシュボードの_「管理」_ページの_「バックアップ (Backups)」_タブから、バックアップの作成とダウンロードを行えます。 日次、週次、月次、オンデマンドでのバックアップを使用できます。 これらは、以下のスケジュールに従って保持されます。

バックアップ・タイプ|保持スケジュール
----------|-----------
日次|日次バックアップは 7 日間保持されます
週次|週次バックアップは 4 週間保持されます
月次|月次バックアップは 3 カ月保持されます
オンデマンド|オンデマンド・バックアップは 1 つ保持されます。 保持されるバックアップは、常に最新のオンデマンド・バックアップです。
{: caption="表 1. バックアップ保持スケジュール" caption-side="top"}

バックアップ・スケジュールと保持ポリシーは固定されています。 保持スケジュールで許可されているよりも多くのバックアップを保持する必要がある場合は、バックアップをダウンロードし、ビジネス要件に従ってアーカイブを保持する必要があります。

## 既存のバックアップの表示

データベースの日次バックアップは自動的にスケジュールされます。 既存のバックアップを表示するには、サービス・ダッシュボードの*「管理」*ページに移動します。 

  ![バックアップ](./images/scylla-backups-show.png "サービス・ダッシュボードのバックアップのリスト")

対応する行をクリックして、選択可能バックアップのオプションを展開します。

  ![バックアップのオプション](./images/scylla-backups-options.png "バックアップのオプション。") 

### API を使用した既存のバックアップの表示

バックアップのリストを `GET /2016-07/deployments/:id/backups` エンドポイントで取得できます。 ファウンデーション・エンドポイントとサービス・インスタンス ID は、両方ともサービスの _「概要」_に表示されます。 例: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```    

## オンデマンドでのバックアップ作成

スケジュールされたバックアップだけでなく、バックアップを手作業で作成することができます。 手動でバックアップを作成する場合は、サービス・ダッシュボードの*「管理」*ページに移動して、*「今すぐバックアップ」*をクリックします。

### API を使用したバックアップの作成

backups エンドポイントに POST 要求 `POST /2016-07/deployments/:id/backups` を送信して、手動でバックアップを開始できます。 この要求はただちに戻り、実行中のバックアップのレシピ ID と情報を返します。 バックアップを使用するには、その前に、バックアップが完了したかどうかを backups エンドポイントで確認し、バックアップ ID を見つける必要があります。 `GET /2016-07/deployments/:id/backups/` を使用します。

## バックアップのリストア
新規サービス・インスタンスにバックアップをリストアするには、以下のようにします。

1. 既存のバックアップを表示するためのステップに従います。
2. 対応する行をクリックして、ダウンロードするバックアップのオプションを展開します。
3. **「リストア」**ボタンをクリックします。 復元が開始されたことを示すメッセージが表示されます。 新しいサービス・インスタンスには「scylla-restore-[timestamp]」という名前が自動的に付けられます。プロビジョニングが開始されるとダッシュボードに名前が表示されます。

### {{site.data.keyword.cloud_notm}} CLI を使用したリストア

以下の手順に従って、{{site.data.keyword.cloud_notm}} CLI を使用して、実行中の Scylla サービスのバックアップを新規 Scylla サービスにリストアします。

1. 必要に応じて、[ CLI をダウンロードしてインストールします](https://console.{DomainName}/docs/cli/index.html#overview)。 
2. サービスの_「バックアップ」_ページで、リストアするバックアップを見つけ、バックアップ ID をコピーします。  
  **または**  
  `GET /2016-07/deployments/:id/backups` を使用して、Compose API でバックアップとその ID を見つけます。 ファウンデーション・エンドポイントとサービス・インスタンス ID は、両方ともサービスの _「概要」_に表示されます。 例: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  応答には、そのサービス・インスタンスに使用可能なすべてのバックアップのリストが含まれています。 リストアするバックアップを選択し、その ID をコピーします。

3. 適切なアカウントと資格情報を使用してログインします。 `ibmcloud login` (または、すべてのログイン・オプションを表示するには、`ibmcloud login -help` を使用)

4. 組織とスペースに切り替えます (`ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`)。

5. `service create` コマンドを使用して新規サービスをプロビジョンし、リストアするソース・サービスと特定のバックアップを JSON オブジェクトで指定します。 例:
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  _SERVICE_ フィールドでは「compose-for-scylladb」を使用します。_PLAN_ フィールドでは、環境に応じて Standard または Enterprise を選択します。 _SERVICE\_INSTANCE\_NAME_ は、新規サービスの名前を入力する場所です。 _source\_service\_instance\_id_ は、バックアップのソースのサービス・インスタンス ID です。`ibmcloud cf service DISPLAY_NAME --guid` を実行して取得できます。ここで、_DISPLAY\_NAME_ はバックアップ元の Scylla サービスの名前です。 
  
  また、エンタープライズ・ユーザーは、`"cluster_id": "$CLUSTER_ID"` パラメーターを使用して、デプロイ先のクラスターも JSON オブジェクトに指定する必要があります。
  
### 新規バージョンへのマイグレーション

一部のメジャー・バージョンのアップグレードは、現在実行中のデプロイメントでは使用できません。 アップグレードしたバージョンを実行する新規サービスをプロビジョンしてから、バックアップを使用してそのサービスにデータをマイグレーションする必要があります。 このプロセスは、アップグレード先のバージョンを指定することを除いて、[バックアップのリストア](#restoring-a-backup)と同じです。

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

例えば、古いバージョンの {{site.data.keyword.composeForScyllaDB}} サービスを Scylla 2.0.3 を実行する新規サービスにリストアする場合は、次のようにします。
```
ibmcloud service create compose-for-scylladb Standard migrated_scylla -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"2.0.3"  }'

