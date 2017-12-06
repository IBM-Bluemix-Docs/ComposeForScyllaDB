---

copyright:
  years: 2017
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 外部アプリケーションの接続
{: #connecting-external-app}

{{site.data.keyword.composeForScyllaDB_full}} に接続するために必要な情報は、{{site.data.keyword.composeForPostgreSQL_full}} サービスの*「概要」*ページに表示されます。

`cqlsh` を使用して、{{site.data.keyword.composeForScyllaDB}} に直接接続できます。このツールをローカル・デバイスに用意する方法は、ローカル・プラットフォームに応じて異なります。最も簡単な方法は、プラットフォームに応じた方式で最新の Cassandra リリース (最新バージョンはバージョン 2.1.8 (Scylla) をまだサポートしています) をインストールし、組み込みの `cqlsh` コマンドを使用する方法です。

以下は、Mac の場合の例です。


1. `brew install cassandra` と入力し、Cassandra を `homebrew` を使用してインストールします。
2. 「概要」ページからコマンドをコピーします。

  ![`cqlsh` 接続ストリングの例。](./cqlsh_connection_string "cqlsh 接続ストリングの例。")

3. コマンドをシェルに貼り付けて実行します。

  ![`cqlsh` シェル。](./cqlsh_shell.png "cqlsh シェル")

4. `HELP` と入力すると、シェルに多数の機能があることがわかります。さらに便利なことに、これらのコマンドはすべて `TAB` による入力補完機能に対応しています。
5. `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>` と入力します。複製クラスとして選択できるものが表示されます。
6. クラスターは複数のデータ・センターにまたがっていないので、ここでは `SimpleStrategy` を選択できます。
7. もう一度 `<TAB><TAB>` と押して、replication_factor に 3 を入力します。次に、`}` で中括弧を閉じ、ステートメントを `;<enter>` で終了します。

  これで、最初の KEYSPACE が作成され、データをクラスター内の 3 つのすべてのノードに複製することがデフォルトに設定されました。

8. 次のようにしてキースペースを使用できます。

  ```sql
  USE my_new_keyspace;
  ```

  コマンド・プロンプトがデフォルトでキースペースを使用することがシェルに表示されます。

  ![`CREATE KEYSPACE` と `USE` を実行します。](./images/running_create_keyspace_use.png "`CREATE KEYSPACE` と `USE` を実行します。")

  キースペースはすべての表に必要です。この場合にシェルで表を作成すると、デフォルトで `my_new_keyspace` に設定されます。

  Scylla/Cassandra は、SQL に非常に似たスキーマ言語を持つように進化しましたが、これはあまり当てはまりません。RDBMS とは異なり、ここでの行は、むしろキー値の検索に似ています。ここでは、例外的に柔軟な値のスキーマを定義することにします。

9. 次の `CREATE TABLE` コマンドをシェルに入力して、例でデータを設定する場所を作成します。

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

## JVM からの接続

Cassandra の最も高機能なドライバーは Java ドライバーです。Cassandra が Java で作成されたことを考えれば当然のことです。その次は [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted) スクリプトです。どの JVM 言語を利用する人にとっても、Groovy から好みの言語に変換することは比較的簡単です。

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
@Grab('org.slf4j:slf4j-log4j12')

import com.datastax.driver.core.BoundStatement
import com.datastax.driver.core.Cluster
import com.datastax.driver.core.Host
import com.datastax.driver.core.PreparedStatement
import com.datastax.driver.core.Row
import com.datastax.driver.core.Session

import static java.util.UUID.randomUUID

Cluster cluster = Cluster.builder()
    .addContactPointsWithPorts(
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15399 ),
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15401 ),
        new InetSocketAddress("aws-us-east-1-portal6.dblayer.com", 15400 )
    )
    .withCredentials("scylla", "XOEDTTBPZGYAZIQD")
    .build()

Session session = cluster.connect("my_new_keyspace")

PreparedStatement myPreparedInsert = session.prepare(
  """INSERT INTO my_new_table(my_table_id, last_name, first_name)
     VALUES (?,?,?)""")

BoundStatement myInsert = myPreparedInsert
    .bind(randomUUID(), "Hutton", "Hays")

session.execute(myInsert)

session.close()
cluster.close()
```

始めに、Cassandra の最新のドライバーをプルします。

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

すべてのインポートの後に、`Cluster.builder()` を使用して構成を設定します。`ContactPoint` の 1 つのみが接続に使用されます。この接続から、クラスター内の他のノードが検出されます。`connect` でこの `ContactPoint` に到達できない場合は別のものが使用されます。これが 3 つのすべてを追加している理由です。

`PreparedStatement` は同じ名前の他の DB 機能と似ているので、理解しやすいはずです。ステートメントを解析し、何度も使用できるようにサーバーに保持します。その次の `bind` 呼び出しと `execute` 呼び出しは、データを設定し、サーバーに送信して実際に実行します。1 回のみ実行する場合は、より簡単な方法を使用できますが、このような便利な機能を強調しておくことは良いことです。

スクリプトが正常に実行されたことを検証するために、`cqlsh` に戻り、次のように表を照会します。
![`cqlsh` での `SELECT` の結果](./images/results_select_java.png "Select の結果")

## Python からの接続

Java 以外の言語のサポートも非常に強固です。Python が良い例です。`cqlsh` は Python でも作成されています。このため、ここでいうサポートとは最新であるだけではないので注意してください。

```shell
pip install cassandra-driver
```

これにより、Python パッケージ・マネージャー `pip` を使用してドライバーをプルします。以下は、ステートメントを準備して挿入を実行する Java コードに非常に似た動作になります。

```python
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider
import uuid

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='XOEDTTBPZGYAZIQD')

cluster = Cluster(
            contact_points = ["aws-us-east-1-portal9.dblayer.com"],
            port = 15401,
            auth_provider = auth_provider)

session = cluster.connect('my_new_keyspace')

my_prepared_insert = session.prepare("""
    INSERT INTO my_new_table(my_table_id, first_name, last_name)
    VALUES (?, ?, ?)""")

session.execute(my_prepared_insert, [uuid.uuid4(), 'Snake', 'Hutton'])
```

もう一度検証するために、同じ `SELECT` コマンドを実行します。

![`cqlsh` での `SELECT` からの結果。](./images/results_select_python.png "Select からの結果")

## Node.js からの接続

ノード・パッケージ・マネージャー (npm) を使用して、ドライバーと必要な `uuid` ライブラリーをインストールします。

```shell
npm install cassandra-driver
npm install uuid
```

 このコードは前の両方の例に似ています。

```javascript
var cassandra = require('cassandra-driver')
var authProvider = new cassandra.auth.PlainTextAuthProvider('scylla', 'XOEDTTBPZGYAZIQD')
var uuid = require('uuid')

client = new cassandra.Client({
                        contactPoints: [
                          "aws-us-east-1-portal9.dblayer.com:15399",
                          "aws-us-east-1-portal9.dblayer.com:15401",
                          "aws-us-east-1-portal6.dblayer.com:15400"
                        ],
                        keyspace: 'my_new_keyspace',
                        authProvider: authProvider});

client.execute("INSERT INTO my_new_table(my_table_id, first_name, last_name) VALUES(?,?,?)",
               [uuid.v4(), "V8", "Hutton"],
               { prepare: true },
               function(err, result) {
                 if(err) { console.error(err); }
                 console.log("success")
               });

```

このコードも同じく接続し、挿入ステートメントを準備して実行します。SELECT コマンドを使用してコードを検証します。

![`cqlsh` での `SELECT` からの結果。](./images/results_select_node.png "Select からの結果")
