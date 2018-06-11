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

# cqlsh の使用
{: #using-cqlsh}

`cqlsh` を使用して、{{site.data.keyword.composeForScyllaDB_full}} に直接接続できます。 これは多くの方法でインストールできます。Python アプリケーションであるため、`pip install cqlsh` を実行して最新バージョンのコマンドをインストールできます。この資料の作成時点では、`cqlsh` は Python 2.7 のプログラムであり、Python 3 では実行できません。

Cassandra の完全なディストリビューションから `cqlsh` を取得することもできます。これは [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) から入手できます。ここでは、バイナリー、ソース、および、Debian (apt) と Red Hat (RPM) Linux ディストリビューションに Cassandra をインストールする方法が提供されています。バイナリー・リリースをダウンロードし、ファイルをディレクトリーに解凍します。そのディレクトリーに `cd` で移動すると、`bin` ディレクトリーに `cqlsh` があります。

このツールをローカル・デバイスに用意する方法は、ローカル・プラットフォームに応じて異なります。 最も簡単な方法は、プラットフォームに応じた方式で最新の Cassandra リリース (最新バージョンはバージョン 2.1.8 (Scylla) をまだサポートしています) をインストールし、組み込みの `cqlsh` コマンドを使用する方法です。 

例えば Mac では、`brew install cassandra` と入力し、`homebrew` を使用して Cassandra をインストールします。

## cqlshrc ファイルについて
`cqlsh` コマンドを実行すると、デフォルトでは、ファイル `$HOME/.cassandra/cqlshrc` がロードされます。このファイルにより、コマンド・ラインで設定できない多くのパラメーターを設定できます。[Apache の cqlshrc の例](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample)を参照してください。このデフォルトのファイルを編集してパラメーターを設定できます。それらは、コマンド・ラインでオーバーライドしない限り、すべてのセッションに適用されます。

さまざまな環境で複数の Scylla インスタンスを実行する場合は、別々に `cqlshrc` ファイルを作成し、コマンドに `--cqlshrc=[filename]` を追加して `cqlsh` で使用することができます。

### {{site.data.keyword.composeForScyllaDB}} の cqlshrc
以下に、サービスに接続するための最低限の `cqlshrc` ファイルの例を示します。
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

サービスの_接続ストリング_から取得した情報を入力し、そのファイルを example.cqlshrc として保存します。
コマンド・ラインから `cqlsh --ssl --cqlshrc=example.cqlshrc` を実行して、Scylla データベースにログインします。`cqlshrc` に `ssl=true` を指定することもできますが、指定しても無視されるようなので、`--ssl` フラグを使用する必要がある点に注意してください。

`cqlshrc` ファイルでは SSL バージョンを設定できないので、TLSv1.2 を使用するように環境を設定するか、SSL_VERSION=TLSv1_2 を `cqlsh` 接続コマンドの前に追加する必要があります。

## TLSv1.2

`cqlsh` は、暗号化される接続をネゴシエーションするときに、デフォルトでは TLSv1.2 を使用しません。Compose Scylla デプロイメントに正常に接続するには、`cqlsh` を実行する環境で TLS バージョンを指定する必要があります。`export SSL_VERSION=TLSv1_2` を使用して SSL_VERSION を環境で設定するか、`cqlsh` 接続コマンドの前に `SSL_VERSION=TLSv1_2` を入力してください。

## 検証なしの接続

TLS /SSL を使用して Scylla に接続する最も簡単な方法は、リモート・ホストを検証しないことです。これは、実稼働環境には推奨されません。 デフォルトでは、検証は有効です。検証は環境変数 `SSL_VALIDATE` または cqlshrc ファイルの SSL 設定によって制御されます。検証を無効にするには、`export SSL_VALIDATE=false` を実行して、環境の `SSL_VALIDATE` を false に設定します。


または、以下を cqlshrc ファイルに追加することもできます。

```
[ssl]  
validate = false
```

1 回の `cqlsh` コマンドに限り無効にするには、`SSL_VALIDATE=false` を `cqlsh` 接続コマンドの前に入力します。 

証明書を検証しない場合でも、`cqlsh` に TLSv1.2 を使用するように指示する必要があります。環境で ssl バージョンを設定していない場合に、検証しないようにするには、次の例のような接続になります。

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 証明書の取得と使用

cqlsh を使用してリモート・ホストを検証する場合は、証明書を取得し、アクセス可能な場所に保存してから、証明書のパスを cqlsh に指定する必要があります。証明書のコピーが [「Scylla および証明書」](doc:scylla-and-certificates)ページに表示され、その内容とソースに関する詳細が表示されます。証明書ファイルを作成するか、コピーをローカルに保存します。 

環境変数 `SSL_CERTFILE` に、保存した証明書のパスとファイル名を設定して、それ以降のすべての `cqlsh` 呼び出しで使用できるようにします。 

あるいは、以下を cqlshrc ファイルに追加することもできます。
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

1 回の `cqlsh` コマンドに限り特定の証明書を使用する場合は、cqlsh 接続コマンドの先頭に `SSL_CERTFILE = ~/path_to_your/lechain.pem` と入力します。例:

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## cqlsh の使用

`HELP` と入力すると、シェルに多数の機能があることがわかります。 さらに便利なことに、これらのコマンドはすべて `TAB` による入力補完機能に対応しています。例:
1. `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>` と入力します。 複製クラスとして選択できるものが表示されます。
2. クラスターは複数のデータ・センターにまたがっていないので、ここでは `SimpleStrategy` を選択できます。
3. もう一度 `<TAB><TAB>` と押して、replication_factor に 3 を入力します。 次に、`}` で中括弧を閉じ、ステートメントを `;<enter>` で終了します。

これで、最初の KEYSPACE が作成され、データをクラスター内の 3 つのすべてのノードに複製することがデフォルトに設定されました。
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

次のようにしてキースペースを使用できます。
```sql 
USE my_new_keyspace;
```
コマンド・プロンプトがデフォルトでキースペースを使用することがシェルに表示されます。キースペースはすべての表に必要です。

次の `CREATE TABLE` コマンドをシェルに入力して、サンプルを取り込む表を作成します。
```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
