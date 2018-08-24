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

# cqlsh の使用
{: #using-cqlsh}

`cqlsh` を使用して、{{site.data.keyword.composeForScyllaDB_full}} に直接接続することができます。

この資料の作成時点では、`cqlsh` は Python 2.7 のプログラムであり、Python 3 では実行できません。
{: .tip}

`cqlsh` は Python アプリケーションであるため、`pip install cqlsh` を実行して最新バージョンのコマンドをインストールできます。

`cqlsh` は、[http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) からダウンロードして、Cassandra の完全なディストリビューションから取得することもできます。そこでは、バイナリーとソースが提供されており、Debian (apt) および Red Hat (RPM) の Linux ディストリビューションへの Cassandra のインストール手順が示されています。 バイナリー・リリースをダウンロードし、ファイルをディレクトリーに解凍します。 そのディレクトリーに `cd` で移動すると、`bin` ディレクトリーに `cqlsh` があります。

このツールをローカル・デバイスに用意する方法は、ローカル・オペレーティング・システムに応じて異なります。 最も簡単な方法は、ご使用のオペレーティング・システムに合った方式で最新の Cassandra リリース (最新バージョンはバージョン 2.1.8 (Scylla) をまだサポートしています) をインストールし、組み込みの `cqlsh` コマンドを使用する方法です。 

例えば Mac では、`brew install cassandra` と入力することで、`homebrew` を使用して Cassandra をインストールできます。

## cqlshrc ファイル

`cqlsh` コマンドを実行すると、デフォルトで `$HOME/.cassandra/cqlshrc` がロードされます。これにより、コマンド・ラインの settable で設定されない多くのパラメーターを設定できます。 このデフォルトのファイルを編集してパラメーターを設定できます。加えた変更は、コマンド・ラインからオーバーライドしない限り、すべてのセッションに適用されます。

さまざまな環境で複数の Scylla インスタンスを実行する場合は、別々に `cqlshrc` ファイルを作成し、コマンドに `--cqlshrc=[filename]` を追加して使用します。


完全な cqlshrc ファイルの例については、[cqlshrc の Apache サンプル](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample)を参照してください。 

### {{site.data.keyword.composeForScyllaDB}} の cqlshrc

サービスに接続するための最低限の `cqlshrc` ファイルは、以下のようになります。

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

サービスの_接続ストリング_ に示された情報を入力し、そのファイルを `example.cqlshrc` として保存します。

これで、コマンド・ラインから `cqlsh --ssl --cqlshrc=example.cqlshrc` を実行して、Scylla データベースにログインできるようになります。 `cqlshrc` には `ssl=true` を指定するためのオプションがありますが、指定しても無視されるようなので、`--ssl` フラグを使用する必要がある点に注意してください。

## TLSv1.2

`cqlsh` は、暗号化される接続をネゴシエーションするときに、デフォルトでは TLSv1.2 を使用しません。 Compose Scylla デプロイメントに正常に接続するには、`cqlsh` を実行する環境で TLS バージョンを指定する必要があります。 最大限の互換性を得るため (TLSv1.3 への将来の対応も加味)、ご使用の環境の `SSL_VERSION` を SSLv23 に設定します。これを行うには、`export SSL_VERSION=SSLv23` を使用するか、`cqlsh` 接続コマンドの前に `SSL_VERSION=SSLv23` を配置します。

バージョンを明示的に TLSv1.2 に設定することもできます。これを行うには、`export SSL_VERSION=TLSv1_2` を使用するか、`cqlsh` 接続コマンドの前に `SSL_VERSION=TLSv1_2` を配置します。

## 検証なしの接続

検証はデフォルトで有効になっていますが、リモート・ホストを検証せずに TLS/SSL を使用して Scylla に接続することができます。 ただし、これは実動システムでは推奨されていません。 検証は環境変数 `SSL_VALIDATE`、または cqlshrc ファイルの SSL 設定によって制御されます。 検証を無効にするには、ご使用の環境の `SSL_VALIDATE` を false に設定します (`export SSL_VALIDATE=false`)。

別の方法として、cqlshrc ファイルを編集して検証を無効にすることもできます。

```
[ssl]  
validate = false
```

1 回の `cqlsh` コマンドで検証を無効にする場合は、`cqlsh` 接続コマンドの前に `SSL_VALIDATE=false` を含めます。 

検証が有効でない場合も、`cqlsh` に TLSv1.2 を使用するように指示する必要があります。 環境で SSL バージョンを設定していない場合に、検証しないようにするには、次の例のような接続になります。

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 証明書の取得と使用

cqlsh を使用してリモート・ホストを検証する場合は、証明書を取得し、アクセス可能な場所に保存してから、証明書のパスを cqlsh に指定する必要があります。

環境変数 `SSL_CERTFILE` に、保存した証明書のパスとファイル名を設定して、それ以降のすべての `cqlsh` 呼び出しで使用できるようにします。 

または、以下を cqlshrc ファイルに追加することもできます。

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

1 回の `cqlsh` コマンドに限り特定の証明書を使用する場合は、cqlsh 接続コマンドの先頭に `SSL_CERTFILE = ~/path_to_your/lechain.pem` と入力します。 例:

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## cqlsh の概説

`HELP` と入力すると、シェルに多数の機能があることがわかります。 コマンドのすべてに、`TAB` による入力補完機能も備わっています。 例:

1. `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>` と入力します。 複製クラスとして選択できるものが表示されます。
2. クラスターは複数のデータ・センターにまたがっていないので、ここでは `SimpleStrategy` を選択します。
3. もう一度 `<TAB><TAB>` と押して、`replication_factor` 値に 3 を入力します。 次に、`}` で中括弧を閉じ、ステートメントを `;<enter>` で終了します。

最初の KEYSPACE が作成され、データをクラスター内の 3 つのすべてのノードに複製する操作がデフォルトに設定されました。

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

コマンド・プロンプトがデフォルトでキースペースを使用することがシェルに表示されます。 キースペースはすべての表に必要です。

次の `CREATE TABLE` コマンドをシェルに入力して、サンプルを取り込む表を作成します。

```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
