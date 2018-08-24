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

# 连接外部应用程序
{: #connecting-external-app}

您可以在 {{site.data.keyword.composeForPostgreSQL_full}} 服务的*概述*页面中找到连接到 {{site.data.keyword.composeForScyllaDB_full}} 所需的信息。

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

要连接 Python 应用程序，请使用 [DataStax Python Driver](https://github.com/datastax/python-driver)。安装可通过 pip 执行：

```shell
pip install cassandra-driver
```

这将使用 Python 软件包管理器 `pip` 拉入驱动程序。驱动程序需要在启用 TLS/SSL 时要使用的证书，并且将证书用于服务的指示信息位于[使用 LE 证书](./scylla-certificates.html)页面上。驱动程序所需的其他所有信息都位于服务_概述_的_连接字符串_中。

以下操作的执行与预编译语句和执行插入操作的 Java 代码非常相似：

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
