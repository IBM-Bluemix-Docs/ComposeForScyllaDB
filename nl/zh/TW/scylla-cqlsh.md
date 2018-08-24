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

您可以使用 `cqlsh` 直接連接至 {{site.data.keyword.composeForScyllaDB_full}}。

在本文撰寫時，`cqlsh` 是 Python 2.7 程式，且不會與 Python 3 搭配執行。
{: .tip}

因為 `cqlsh` 是 Python 應用程式，您可以執行 `pip install cqlsh` 安裝指令的最新版本。

您也可以從 Cassandra 的完整發行套件取得 `cqlsh`，方法是從 [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) 下載它，這裡提供二進位檔、原始檔，以及如何在 Debian (apt) 和 Red Hat (RPM) Linux 發行套件上安裝 Cassandra 的指示。請下載二進位檔版本，並將檔案解壓縮至目錄。然後，`cd` 到該目錄，您可在 `bin` 目錄中找到 `cqlsh`。

在本端裝置上取得此工具的方式，視您的本端作業系統而定。最簡單的是使用您作業系統所適用的方法來安裝最新的 Cassandra 版本（最新版本仍支援 2.1.8 版，即所謂的 Scylla），然後使用內建的 `cqlsh` 指令。 

例如，在 Mac 上，您可以鍵入 `brew install cassandra` 以使用 `homebrew` 安裝 Cassandra。

## cqlshrc 檔案

依預設，當 `cqlsh` 指令執行時，它會載入 `$HOME/.cassandra/cqlshrc`，這可以設定許多無法在指令行中設定的參數。您可以編輯預設檔案來設定參數，您所做的任何變更都適用於所有階段作業，除非從指令行置換。

如果您在不同的環境中執行多個 Scylla 實例，則可以建立個別的 `cqlshrc` 檔案，並將 `--cqlshrc=[filename]` 新增至您的指令來使用它。


如需完整 cqlshrc 檔案的範例，請參閱 [Apache 的 cqlshrc 範例](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample)。 

### {{site.data.keyword.composeForScyllaDB}} 的 cqlshrc

連接至您的服務的最小 `cqlshrc` 檔案看起來如下：

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

請輸入您服務之_連線字串_ 中所提供的資訊，並將檔案儲存為 `example.cqlshrc`。

您現在可以從指令行執行 `cqlsh --ssl --cqlshrc=example.cqlshrc`，以登入 Scylla 資料庫。請注意，即使 `cqlshrc` 具有指定 `ssl=true` 的選項，似乎仍會被忽略，因此您必須使用 `--ssl` 旗標。

## TLSv1.2

協議加密連線時，`cqlsh` 未預設為 TLSv1.2。為了順利連接至 Compose Scylla 部署，必須在執行 `cqlsh` 的環境中指定 TLS 版本。若要取得最大的相容性（以及對於 TLSv1.3 的一點未來保證），請將環境中的 `SSL_VERSION` 設為 SSLv23，方法是使用 `export SSL_VERSION=SSLv23` 或將 `SSL_VERSION=SSLv23` 放在 `cqlsh` 連線指令之前。

您也可以將版本特別設定為 TLSv1.2，方法是使用 `export SSL_VERSION=TLSv1_2` 或將 `SSL_VERSION=TLSv1_2` 放在 `cqlsh` 連線指令之前。

## 連接但不進行驗證

依預設會啟用驗證，但您可以使用 TLS/SSL 連接至 Scylla 而不驗證遠端主機。不過，這不建議用於正式作業系統。驗證是透過環境變數 `SSL_VALIDATE` 或 cqlshrc 檔案中的 SSL 設定所控制。請在您的環境中，將 `SSL_VALIDATE` 設為 false (`export SSL_VALIDATE=false`) 以停用驗證。

或者，您可以編輯 cqlshrc 檔案來停用驗證。

```
[ssl]
validate = false
```

如果您要針對單一 `cqlsh` 指令停用驗證，則請將 `SSL_VALIDATE=false` 包含在 `cqlsh` 連線指令的前面。 

即使未啟用驗證，您仍然需要告知 `cqlsh` 使用 TLSv1.2。如果您尚未在環境中設定 SSL 版本，而且不想要驗證，則連線應該與下列範例類似：

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 取得及使用憑證

如果您要使用 cqlsh 並驗證遠端主機，則需要取得憑證，並將它儲存至某個可存取的位置，然後提供憑證路徑給 cqlsh。

請將環境變數 `SSL_CERTFILE` 設為已儲存憑證的路徑及檔名，以便為 `cqlsh` 的所有後續呼叫啟用它。 

或者，您可以將下列項目新增至 cqlshrc 檔案。

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

如果您只要針對單一 `cqlsh` 指令使用特定憑證，則請將 `SSL_CERTFILE = ~/path_to_your/lechain.pem` 放在 cqlsh 連線指令的開頭。例如：

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 開始使用 cqlsh

如果您鍵入 `HELP`，可以看到 Shell 具有許多功能。甚至所有指令也都有 `TAB` 完成。例如：

1. 鍵入 `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`。您應該會看到抄寫類別的選項。
2. 在這裡選擇 `SimpleStrategy`，因為叢集不會跨越多個資料中心。
3. 再次按下 `<TAB><TAB>`，並對 `replication_factor` 值輸入 3。然後，以大括弧 `}` 括住，並以 `;<enter>` 完成陳述式。

您建立了第一個 KEYSPACE，並將它預設為把您的資料抄寫至叢集裡的全部三個節點。

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

現在，您可以使用索引鍵空間：

```sql 
USE my_new_keyspace;
```

您的 Shell 現在會顯示，您的指令提示依預設正在使用您的索引鍵空間。每個表格都必須有一個索引鍵空間。

在 Shell 中鍵入下列 `CREATE TABLE` 指令，以建立要移入範例的表格。

```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
