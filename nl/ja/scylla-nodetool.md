---

copyright:
  years: 2017,2018
lastupdated: "2017-12-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# nodetool の使用
{: #using-nodetool}

nodetool で {{site.data.keyword.composeForScyllaDB_full}} サービスを管理するには、以下の作業が必要です。
1. SSH 鍵を追加します。
2. 接続ストリングの_「Socks」_タブで SSH コマンドを使用してトンネルを開きます。

トンネルが開いたら、接続ストリングの_「Nodetool」`タブに示されている形式で `nodetool_ コマンドを使用できます。指定したコマンドから、Scylla クラスターの状況が返されます。

## 使用可能な nodetool コマンド
nodetool にはさまざまなコマンドがありますが、クラスターの自動保守にダメージを与える可能性のある状況を回避するために、使用できるコマンドが制限されています。使用可能なコマンドは以下のとおりです。

コマンド|機能
----------|-----------
cfhistograms|表に関する統計 (SSTable の数、読み取り/書き込み待ち時間、パーティション・サイズ、列カウントなど) を提供します。
cfstats|列ファミリーに関する詳細な診断情報を提供します。
cleanup|ノードのものではなくなったキーの即時クリーンアップを起動します。
compact|1 つ以上の列ファミリーで (メジャー) 圧縮を強制実行します。
compactionhistory|圧縮の履歴を出力します。
compactionstats|圧縮に関する統計を出力します。
describecluster|クラスターの名前、スニッチ、パーティショナー、スキーマのバージョンを出力します。
describering <keyspace>|指定のキースペースのトークン範囲情報を表示します。
flush|1 つ以上の列ファミリーをフラッシュします。
help|ヘルプを出力します。
getendpoints <keyspace> <cfname> <key>|キーを所有するエンドポイントを出力します。
gossipinfo|クラスターのゴシップ情報を表示します。
info|ノード情報を出力します。
move <new token>|トークンリング上のノードを新しいトークンに移動します。
netstats|指定のホスト (デフォルトでは接続元のノード) に関するネットワーク情報を出力します。
proxyhistograms|ネットワーク操作の統計ヒストグラムを出力します。
repair|1 つ以上の列ファミリーを修復します。
ring|トークンリング情報を表示します。
status|クラスター情報を出力します。
statusbackup|ScyllaDB バックアップの状況を出力します。
statusbinary|ネイティブ・トランスポート (バイナリー・プロトコル) の状況を出力します。
statusgossip|ゴシップの状況を出力します。
version|DB バージョンを出力します。
{: caption="表 1. 使用可能な nodetool コマンド" caption-side="top"}


## ブロックされる nodetool コマンド
nodetool の次のコマンドは、{{site.data.keyword.composeForScyllaDB}} に対して使用しようとすると、ブロックされて使用できません。

- clearsnapshot
- decommision
- disablebackup
- disablebinary
- disablegossip
- drain
- enablebackup
- enablebinary
- enablegossip
- getlogginglevels
- listsnapshots
- rebuild
- refresh
- removenode
- setlogginglevel
- settraceprobability
- snapshot
- stop
