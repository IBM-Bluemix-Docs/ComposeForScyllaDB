---

copyright:
  years: 2018
lastupdated: "2018-05-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 使用 cqlsh
{: #using-cqlsh}

您可以使用 `cqlsh` 直接连接到 {{site.data.keyword.composeForScyllaDB_full}}。

在编写本文时，`cqlsh` 是 Python 2.7 程序，因此未使用 Python 3 运行。
{: .tip}

因为 `cqlsh` 是 Python 应用程序，因此可以运行 `pip install cqlsh` 安装最新版本的命令。

您还可以通过从 [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) 下载 Cassandra 的完整分发版，从中获取 `cqlsh`，其中提供了二进制文件、源代码以及如何在 Debian (apt) 和 Red Hat (RPM) Linux 分发版上安装 Cassandra 的指示信息。下载二进制发行版并将该文件解压到目录。然后，使用 `cd` 命令进入该目录，您将在 `bin` 目录中找到 `cqlsh`。

如何将此工具获取到本地设备上取决于您的本地操作系统。最简单的方法是使用适用于您操作系统的方法安装最新的 Cassandra 发行版（最新版本仍支持 V2.1.8，即 Scylla），然后使用内置 `cqlsh` 命令。 

例如，在 Mac 上，可以使用 `homebrew` 通过输入 `brew install cassandra` 安装 Cassandra。

## cqlshrc 文件

`cqlsh` 命令运行时，缺省情况下会装入 `$HOME/.cassandra/cqlshrc`，此文件可设置无法在命令行中设置的多个参数。您可以编辑缺省文件来设置参数，并且您所做的任何更改都将应用于所有会话，除非参数被命令行覆盖。

如果在不同的环境中运行多个 Scylla 实例，那么可以通过向命令添加 `--cqlshrc=[filename]` 来创建并使用单独的 `cqlsh` 文件。


要获取完整 cqlshrc 文件的示例，请参阅 [cqlshrc 的 Apache 样本](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample)。 

### 用于 {{site.data.keyword.composeForScyllaDB}} 的 cqlshrc

用于连接到服务的最简 `cqlshrc` 文件类似于以下内容：

```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
version=SSLv23
certfile = ~/path_to_your/lechain.pem
validate = true
```

输入服务的_连接字符串_中提供的信息，并将此文件保存为 `example.cqlshrc`。

现在，您可以通过在命令行中运行 `cqlsh --ssl --cqlshrc=example.cqlshrc` 来登录到 Scylla 数据库。请注意，虽然 `cqlshrc` 具有用于指定 `ssl=true` 的选项，但似乎此选项会被忽略，因此必须使用 `--ssl` 标志。

## TLSv1.2

在协商加密连接时，`cqlsh` 不会缺省为 TLSv1.2。要成功连接到 Compose Scylla 部署，必须在运行 `cqlsh` 的环境中指定 TLS 版本。要获得最大的兼容性（以及针对 TLSv1.3 的一些防过时机制），请使用 `export SSL_VERSION=SSLv23` 或者在 `cqlsh` 连接命令之前放置 `SSL_VERSION=SSLv23`，以将环境中的 `SSL_VERSION` 设置为 SSLv23。

您还可以使用 `export SSL_VERSION=TLSv1_2` 或者在 `cqlsh` 连接命令之前放置 `SSL_VERSION=TLSv1_2`，以将版本明确设置为 TLSv1.2。

## 连接而不验证

缺省情况下验证已启用，但您可以使用 TLS/SSL 来连接到 Scylla，而无需验证远程主机。但是，对于生产系统，建议不要使用此选项。验证由环境变量 `SSL_VALIDATE` 或 cqlshrc 文件中的 SSL 设置控制。在环境中将 `SSL_VALIDATE` 设置为 false (`export SSL_VALIDATE=false`) 以禁用验证。

或者，可以编辑 cqlshrc 文件以禁用验证。

```
[ssl]  
validate = false
```

如果只想要针对单个 `cqlsh` 命令禁用验证，请在 `cqlsh` 连接命令之前包含 `SSL_VALIDATE=false`。 

您仍必须告知 `cqlsh` 使用 TLSv1.2，即使在未启用验证时也是这样。如果未在环境中设置 SSL 版本并且不想验证，那么连接应类似于以下示例：

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 获取并使用证书

如果要使用 cqlsh 并验证远程主机，您需要获取证书，将其保存在可访问的某个位置，然后向 cqlsh 提供证书的路径。

将环境变量 `SSL_CERTFILE` 设置为所保存的证书的路径和文件名以针对 `cqlsh` 的所有后续调用启用此证书。 

或者，您可以将以下项添加到 cqlshrc 文件。

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

如果只想要将特定证书用于单个 `cqlsh` 命令，请在 cqlsh 连接命令起始位置放置 `SSL_CERTFILE = ~/path_to_your/lechain.pem`。例如：

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## cqlsh 入门

如果输入 `HELP`，那么可以看到 shell 具有许多功能。所有命令甚至都还有 `TAB` 键填写功能。例如：

1. 输入 `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. 您应该看到复制类的选项。
2. 在此处选择 `SimpleStrategy`，因为集群不会跨多个数据中心。
3. 再次按下 `<TAB><TAB>` 并输入 3 以表示 `replication_factor` 值。然后，使用 `}` 结束花括号并使用 `;<enter>` 完成语句。

您创建了第一个 KEYSPACE，并将其缺省设置为将数据复制到集群中的所有三个节点。

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
