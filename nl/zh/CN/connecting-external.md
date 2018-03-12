---

copyright:
  years: 2017,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 连接外部应用程序
{: #connecting-external-app}

您可以在 {{site.data.keyword.composeForPostgreSQL_full}} 服务的*概述*页面中找到连接到 {{site.data.keyword.composeForScyllaDB_full}} 所需的信息。

您可以使用 `cqlsh` 直接连接到 {{site.data.keyword.composeForScyllaDB}}。如何将此工具放在本地设备上取决于您的本地平台。最简单的方法是使用适用于您平台的方法安装最新的 Cassandra 版本（最新版本仍支持 V2.1.8，即 Scylla）并使用内置 `cqlsh` 命令。

例如，在 Mac 上：

1. 通过输入 `brew install cassandra` 安装 Cassandra 和 `homebrew`。
2. 从概述页面复制任何命令：

  ![示例“cqlsh”连接字符串。](./cqlsh_connection_string "示例 cqlsh 连接字符串")

3. 将命令粘贴到 shell 中以执行该命令：

  ![“cqlsh”shell。](./cqlsh_shell.png "cqlsh shell")

4. 如果输入 `HELP`，那么可以看到 shell 具有许多功能。更为喜人的是所有这些命令也都补全了 `TAB`。
5. 输入 `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`。您应该看到复制类的选项。
6. 您可以在此处选择 `SimpleStrategy`，因为集群不会跨多个数据中心。
7. 再次按下 `<TAB><TAB>` 并针对 replication_factor 输入 3。然后，使用 `}` 结束花括号并使用 `;<enter>` 完成语句。

  您刚创建了第一个 KEYSPACE，并将其缺省设置为将数据复制到集群中的所有三个节点。

8. 现在，您可以使用键空间：

  ```sql
  USE my_new_keyspace;
  ```

  现在，您的 shell 显示命令提示符缺省情况下使用您的键空间：

  ![运行“CREATE KEYSPACE”和“USE”。](./images/running_create_keyspace_use.png "运行“CREATE KEYSPACE”和“USE”")

  每个表都必须具有一个键空间。在此处的 shell 中创建键空间时，它将缺省为 `my_new_keyspace`。

  Scylla/Cassandra 已经发展成具有类似于 SQL 的模式语言。事实并非如此。与 RDBMS 不同，此处的行更像键值查找。而恰巧值具有我们要定义的灵活模式：

9. 在 shell 中输入以下 `CREATE TABLE` 命令，以创建要填充示例的地方。

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

## 通过 JVM 进行连接

Cassandra 最先进的驱动程序之一是 Java 驱动程序。因此，认为 Cassandra 是使用 Java 编写的合情合理。接下来是 [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted) 脚本。对于那些只利用任何 JVM 语言的人来说，从 Groovy 转换为您选择的语言应该相对简单明了：

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

要开始使用，我们拉入最新的 Cassandra 驱动程序：

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

在所有导入完成之后，使用 `Cluster.builder()` 来构建配置。仅使用其中一个 `ContactPoint` 进行连接。通过该连接，将发现集群中的其他节点。如果该 `ContactPoint` 在 `connect` 上不可抵达，那么将使用另一个 ContactPoint，这就是为什么我们添加全部三个的原因。

您可能对 `PreparedStatement` 很熟悉，因为它们与相同名称的其他数据库功能类似。在服务器准备就绪可以使用时，会不断地对语句进行解析和保留。以下对 `bind` 和 `execute` 的调用会添加数据并将数据发送到服务器以进行实际执行。虽然有更简单的方法进行一次性执行，但最好还是强调这样有用的功能。

要证明脚本工作已返回到 `cqlsh` 并查询表：
![“cqlsh”中“SELECT”的结果。](./images/results_select_java.png "Select 的结果")

## 通过 Python 进行连接

对 Java 以外的语言的支持也非常可靠。Python 就是一个很好的例子。`cqlsh` 甚至是以 Python 编写的。因此，毫无疑问，此处的支持是最新的：

```shell
pip install cassandra-driver
```

这将使用 Python 软件包管理器 `pip` 拉入驱动程序。以下操作的执行与预编译语句和执行插入操作的 Java 代码非常相似：

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

要再次验证，我们将运行相同的 `SELECT` 命令：

![“cqlsh”中“SELECT”的结果。](./images/results_select_python.png "Select 的结果")

## 通过 Node.js 进行连接

使用 Node 软件包管理器 (npm) 来安装驱动程序和所需的 `uuid` 库。

```shell
npm install cassandra-driver
npm install uuid
```

 代码类似于先前的示例：

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

代码会再一次连接、准备并执行插入语句。请使用 SELECT 命令验证代码：

![“cqlsh”中“SELECT”的结果。](./images/results_select_node.png "Select 的结果")
