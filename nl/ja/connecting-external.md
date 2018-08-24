---

copyright:
  years: 2017,2018
lastupdated: "2018-02-28"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 外部アプリケーションの接続
{: #connecting-external-app}

{{site.data.keyword.composeForScyllaDB_full}} に接続するために必要な情報は、{{site.data.keyword.composeForPostgreSQL_full}} サービスの*「概要」*ページに表示されます。

## JVM からの接続

Cassandra の最も高機能なドライバーは Java ドライバーです。 Cassandra が Java で作成されたことを考えれば当然のことです。 その次は [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted) スクリプトです。 どの JVM 言語を利用する人にとっても、Groovy から好みの言語に変換することは比較的簡単です。

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

すべてのインポートの後に、`Cluster.builder()` を使用して構成を設定します。 `ContactPoint` の 1 つのみが接続に使用されます。 この接続から、クラスター内の他のノードが検出されます。 `connect` でこの `ContactPoint` に到達できない場合は別のものが使用されます。これが 3 つのすべてを追加している理由です。

`PreparedStatement` は同じ名前の他の DB 機能と似ているので、理解しやすいはずです。 ステートメントを解析し、何度も使用できるようにサーバーに保持します。 その次の `bind` 呼び出しと `execute` 呼び出しは、データを設定し、サーバーに送信して実際に実行します。 1 回のみ実行する場合は、より簡単な方法を使用できますが、このような便利な機能を強調しておくことは良いことです。

スクリプトが正常に実行されたことを検証するために、`cqlsh` に戻り、次のように表を照会します。
![`cqlsh` での `SELECT` の結果](./images/results_select_java.png "Select の結果")

## Python からの接続

python アプリケーションを接続するには、[DataStax Python Driver](https://github.com/datastax/python-driver) を使用します。 インストールは pip を使用して行うことができます。

```shell
pip install cassandra-driver
```

これにより、Python パッケージ・マネージャー `pip` を使用してドライバーをプルします。 TLS/SSL が有効な場合、ドライバーは証明書の使用を予期します。サービスで証明書を使用する方法については、[LE 証明書の使用](./scylla-certificates.html)のページを参照してください。 ドライバーに必要なその他のすべての情報は、サービスの_「概要」_の_「接続ストリング」_にあります。

以下は、ステートメントを準備して挿入を実行する Java コードに非常に似た動作になります。

```python
import ssl
import uuid
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider

ssl_options = {
    "ca_certs":"/path_to_cert/lecert.pem",
    "ssl_version":ssl.PROTOCOL_TLSv1_2
}

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='your_password')

cluster = Cluster(
            contact_points = [
                "portal-59b813def16d6e0014003b45224-8.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",  
                "portal-59b813def16d6e0014003b47186-6.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",  
                "portal-59b813def16d6e0014003b49208-7.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com"
                ],
            port = 15401,
            auth_provider = auth_provider,
            ssl_options=ssl_options)

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

このコードも同じく接続し、挿入ステートメントを準備して実行します。 SELECT コマンドを使用してコードを検証します。

![`cqlsh` での `SELECT` からの結果。](./images/results_select_node.png "Select からの結果")
