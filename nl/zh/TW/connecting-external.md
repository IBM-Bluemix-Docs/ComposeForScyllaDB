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

# 連接外部應用程式
{: #connecting-external-app}

您可以在 {{site.data.keyword.composeForPostgreSQL_full}} 服務的*概觀* 頁面上，找到連接至 {{site.data.keyword.composeForScyllaDB_full}} 所需的資訊。

您可以使用 `cqlsh` 直接連接至 {{site.data.keyword.composeForScyllaDB}}。在本端裝置上取得此工具的方式，視您的本端平台而定。最簡單的是使用您的平台所適用的方法來安裝最新的 Cassandra 版本（最新版本仍支援 2.1.8 版，即所謂的 Scylla），並使用內建的 `cqlsh` 指令。

例如，在 Mac 上：

1. 鍵入 `brew install cassandra`，透過 `homebrew` 安裝 Cassandra。
2. 從「概觀」頁面複製任何指令：

  ![範例 `cqlsh` 連線字串。](./cqlsh_connection_string "範例 cqlsh 連線字串")

3. 將指令貼至您的 Shell 以執行它：

  ![`cqlsh` Shell。](./cqlsh_shell.png "cqlsh Shell")

4. 如果您鍵入 `HELP`，可以看到 Shell 具有許多功能。更好的是所有這些命令也都以 `TAB` 結束。
5. 鍵入 `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`。您應該會看到抄寫類別的選項。
6. 您可以在這裡選擇 `SimpleStrategy`，因為叢集不會跨越多個資料中心。
7. 再次按下 `<TAB><TAB>`，並對 replication_factor 輸入 3。然後，以大括弧 `}` 括住，並以 `;<enter>` 完成陳述式。

  您剛剛建立了第一個 KEYSPACE，並預設為將資料抄寫至叢集中的所有三個節點。

8. 現在，您可以使用索引鍵空間：

  ```sql
  USE my_new_keyspace;
  ```

  您的 Shell 現在會顯示，您的指令提示依預設正在使用您的索引鍵空間：

  ![執行 `CREATE KEYSPACE` 及 `USE`。](./images/running_create_keyspace_use.png "執行 `CREATE KEYSPACE` 及 `USE`")

  每個表格都必須有一個索引鍵空間。當您在這裡的 Shell 中建立一個索引鍵空間時，它將預設為 `my_new_keyspace`。

  儘管 Sccylla/Cassandra 已發展成具有樣子非常類似 SQL 的綱目語言，實際上並不是這樣。與 RDBMS 不同的是，這裡的列與索引鍵值查閱非常相似。只是剛好此值具有我們即將定義的彈性綱目：

9. 在 Shell 中鍵入下列 `CREATE TABLE` 指令，以建立要移入範例的位置。

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

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

也支援 Java 以外的其他語言。Python 是很棒的範例。`cqlsh` 甚至是以 Python 撰寫的。別搞錯，這裡的支援才是最新的：

```shell
pip install cassandra-driver
```

這會使用 Python 套件管理程式 `pip` 取得驅動程式。下列的執行方式與準備陳述式並執行插入的 Java 程式碼非常類似：

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
