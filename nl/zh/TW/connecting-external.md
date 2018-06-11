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

# 連接外部應用程式
{: #connecting-external-app}

您可以在 {{site.data.keyword.composeForPostgreSQL_full}} 服務的*概觀* 頁面上，找到連接至 {{site.data.keyword.composeForScyllaDB_full}} 所需的資訊。

## 從 JVM 連接

Cassandra 的其中一個最高階驅動程式是 Java 驅動程式。考慮到 Cassandra 是以 Java 撰寫，這是有道理的。以下是 [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted) Script。對於那些只使用從 Groovy 轉換到您所選擇語言之任何 JVM 語言的人來說，應該是相對直接明確：

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

為了開始，我們會取得最新的 Cassandra 驅動程式：

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

在完成所有匯入之後，我們會使用 `Cluster.builder()` 來建置配置。只會使用其中一個 `ContactPoint` 來連接。從該連線中探索叢集中的其他節點。如果在 `connect` 上無法與 `ContactPoint` 聯繫，則會使用另一個，這就是我們新增這三個的原因。

`PreparedStatement` 可能很熟悉，因為它們類似於其他相同名稱的資料庫特性。陳述式會進行剖析並保留在伺服器上，以便可以一再地使用。下列對 `bind` 和 `execute` 的呼叫會將資料移入並傳送至伺服器，以進行實際執行。儘管有更簡單的方法可進行一次性執行，但強調這類有用特性卻是很好的作法。

若要證明 Script 可以運作，請回到您的 `cqlsh` 並查詢表格：
![來自 `cqlsh` 中 `SELECT` 的結果。](./images/results_select_java.png "來自 Select 的結果")

## 從 Python 連接

若要連接 Python 應用程式，請使用 [DataStax Python 驅動程式](https://github.com/datastax/python-driver)。安裝可以透過 pip 執行：

```shell
pip install cassandra-driver
```

這會使用 Python 套件管理程式 `pip` 取得驅動程式。驅動程式預期有在啟用 TLS/SSL 時使用的憑證，而搭配使用憑證與服務的指示位於[使用 LE 憑證](./scylla-certificates.html)頁面上。驅動程式所需的所有其他資訊都位於服務_概觀_ 的_連線字串_ 中。

下列的執行方式與準備陳述式並執行插入的 Java 程式碼非常類似：

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

為了再次驗證，我們將執行相同的 `SELECT` 指令：

![來自 `cqlsh` 中 `SELECT` 的結果。](./images/results_select_python.png "來自 Select 的結果")

## 從 Node.js 連接

使用 Node 套件管理程式 (npm) 來安裝驅動程式，以及所需的 `uuid` 程式庫。

```shell
npm install cassandra-driver
npm install uuid
```

 此程式碼類似於之前的範例：

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

程式碼會再次連接，並準備好執行 insert 陳述式。請使用 SELECT 指令來驗證程式碼：

![來自 `cqlsh` 中 `SELECT` 的結果。](./images/results_select_node.png "來自 Select 的結果")
