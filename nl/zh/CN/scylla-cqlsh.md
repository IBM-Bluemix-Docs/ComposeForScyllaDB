---

copyright:
  years: 2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 cqlsh
{: #using-cqlsh}

您可以使用 `cqlsh` 直接连接到 {{site.data.keyword.composeForScyllaDB_full}}。有多种方法可用于安装。因为它是 Python 应用程序，因此运行 `pip install cqlsh` 将安装最新版本的命令。在编写时，`cqlsh` 是 Python 2.7 程序，并且不使用 Python 3 运行。

还可以从 Cassandra 的完整分发版中获取 `cqlsh`。可以从 [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) 获取，其中提供如何安装 Cassandra on Debian (apt) 和 Red Hat (RPM) Linux 分发版的二进制、源代码和指示信息。下载二进制发行版并将文件解压到目录。然后，使用 `cd` 命令进入此目录，您将在 `bin` 目录中找到 `cqlsh`。

如何将此工具放在本地设备上取决于您的本地平台。最简单的方法是使用适用于您平台的方法安装最新的 Cassandra 版本（最新版本仍支持 V2.1.8，即 Scylla）并使用内置 `cqlsh` 命令。 

例如，在 Mac 上，通过输入 `brew install cassandra` 安装 Cassandra 和 `homebrew`。

## 关于 cqlshrc 文件
在运行时，`cqlsh` 命令装入一个文件，缺省情况下为 `$HOME/.cassandra/cqlshrc`，此文件可设置无法在命令行中设置的多个参数。[Apache cqlshrc 示例](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample)。您可以编辑缺省文件以设置参数。除非在命令行上进行覆盖，否则这些将应用于所有会话。

如果在不同的环境中运行多个 Scylla 实例，那么可创建单独的 `cqlshrc` 文件，并通过向命令添加 `--cqlshrc=[filename]` 以使 `cqlsh` 使用该文件。

### 用于 {{site.data.keyword.composeForScyllaDB}} 的 cqlshrc
这是用于连接到服务的最小示例 `cqlshrc` 文件：
```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

输入从服务的_连接字符串_提供的信息并将此文件保存为 example.cqlshrc。
从命令行运行 `cqlsh --ssl --cqlshrc=example.cqlshrc`，您将登录到 Scylla 数据库。请注意，即使 `cqlshrc` 具有用于指定 `ssl=true` 的选项，但看起来被忽略，因此必须使用 `--ssl` 标志。

无法在 `cqlshrc` 文件中设置 SSL 版本，因此您必须设置环境以使用 TLSv1.2，或者在 `cqlsh` 连接命令的签名附加 SSL_VERSION=TLSv1_2。

## TLSv1.2

在协商加密连接时，`cqlsh` 不会缺省为 TLSv1.2。为成功连接到 Compose Scylla 部署，必须在运行 `cqlsh` 的环境中指定 TLS 版本。使用 `export SSL_VERSION=TLSv1_2` 在环境中设置 SSL_VERSION，或者在 `cqlsh` 连接命令之前放置 `SSL_VERSION=TLSv1_2`。

## 连接而不验证

使用 TLS/SSL 连接到 Scylla 的最简单方法是不验证远程主机。建议不要将此用于生产。缺省情况下，验证处于启用状态。验证由环境变量 `SSL_VALIDATE` 或 cqlshrc 文件中的 SSL 设置控制。在环境中将 `SSL_VALIDATE` 设置为 false 以禁用验证：`export SSL_VALIDATE=false`

或者，您可以将以下项添加到 cqlshrc 文件。

```
[ssl]  
validate = false
```

如果只想要针对单个 `cqlsh` 命令禁用它，请在 `cqlsh` 连接命令前放置 `SSL_VALIDATE=false`。 

您仍必须告知 `cqlsh` 使用 TLSv1.2，即使在不验证证书时也是这样。如果未在环境中设置 SSL 版本并且不想验证，那么连接应类似于以下示例：

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 获取并使用证书

如果要使用 cqlsh 并验证远程主机，您将需要获取证书，将其保存在可访问位置，然后向 cqlsh 提供证书的路径。证书副本位于 [Scylla 和证书](doc:scylla-and-certificates)页面中，还包含有关其内容和源的详细信息。生成证书文件或将副本保存在本地。 

将环境变量 `SSL_CERTFILE` 设置为所保存的证书的路径和文件名以针对 `cqlsh` 的所有后续调用启用此证书。 

或者，您可以将以下项添加到 cqlshrc 文件。
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

如果只想要将特定证书用于单个 `cqlsh` 命令，请在 cqlsh 连接命令起始位置放置 `SSL_CERTFILE = ~/path_to_your/lechain.pem`。例如：

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 使用 cqlsh

如果输入 `HELP`，那么可以看到 shell 具有许多功能。更为喜人的是所有这些命令也都补全了 `TAB`。例如：
1. 输入 `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. 您应该看到复制类的选项。
2. 您可以在此处选择 `SimpleStrategy`，因为集群不会跨多个数据中心。
3. 再次按下 `<TAB><TAB>` 并针对 replication_factor 输入 3。然后，使用 `}` 结束花括号并使用 `;<enter>` 完成语句。

您刚创建了第一个 KEYSPACE，并将其缺省设置为将数据复制到集群中的所有三个节点。
```sql
scylla@cqlsh> CREATE KEYSPACE example_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3 };
scylla@cqlsh> use example_keyspace ;
scylla@cqlsh:example_keyspace> create table names (
                    ... name_id uuid,
                    ... last_name text,
                    ... first_name text,
                    ... PRIMARY KEY(name_id)
                    ... );
scylla@cqlsh:example_keyspace> 
```

现在，您可以使用键空间：
```sql 
  USE my_new_keyspace;
  ```
而且现在，您的 shell 显示命令提示符缺省情况下使用您的键空间。每个表都必须具有一个键空间。

在 shell 中输入以下 `CREATE TABLE` 命令，以创建要填充示例的表。
```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
```
