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

# Usando nodetool
{: #using-nodetool}

Para usar o nodetool para administrar o serviço {{site.data.keyword.composeForScyllaDB_full}}, é necessário primeiro incluir uma chave SSH e, em seguida, abrir um túnel usando o comando SSH na guia _Socks_ das sequências de conexões.

Quando o túnel estiver aberto, será possível usar comandos `nodetool` com o formato mostrado na guia _Nodetool_ das sequências de conexões. O comando fornecido retorna o status do cluster do Scylla.

## Comandos nodetool disponíveis

O Nodetool tem uma ampla faixa de comandos, mas seu uso está limitado a evitar situações que possam danificar a manutenção automatizada do cluster. Os comandos disponíveis são mostrados na tabela.

Comando|Função
----------|-----------
`cfhistograms`|Fornecer estatísticas sobre uma tabela, incluindo o número de SSTables, latência de leitura/gravação, tamanho da partição e contagem de colunas.
`cfstats`|Fornecer diagnósticos detalhados em relação à família da coluna.
`cleanup`|Acionar a limpeza imediata das chaves não mais pertencentes a um nó
`compact`|Forçar uma compactação (principal) em uma ou mais famílias de colunas.
`compactionhistory`|Imprimir o histórico de compactação.
`compactionstats`|Imprimir estatísticas sobre as compactações.
`describecluster`|Imprimir o nome, o snitch, o particionador e a versão do esquema de um cluster.
` describering <keyspace>`|Mostrar as informações de faixas de token de um keyspace.
`flush`|Liberar uma ou mais famílias de colunas.
`help`|Imprimir ajuda.
` getendpoints <keyspace> <cfname> <key>`|Imprimir os terminais que possuem a chave.
`gossipinfo`|Mostrar as informações de gossip para o cluster.
`info`|Imprimir informações do nó.
` move <new token>`|Mover o nó no token-ring para um novo token.
`netstats`|Imprimir informações de rede no host fornecido (nó de conexão por padrão).
`proxyhistograms`|Imprimir histogramas estatísticos para operações de rede.
`repair`|Reparar uma ou mais famílias de colunas.
`ring`|Exibir as informações de token ring.
`:NONE.`|Imprimir informações do cluster.
`statusbackup`|Imprimir o status de backup de ScyllaDB.
`statusbinary`|Imprimir o status do transporte nativo (protocolo binário).
`statusgossip`|Imprimir o status de gossip.
`version`|Imprimir a versão do BD.
{: caption="Tabela 1. Comandos nodetool disponíveis" caption-side="top"}


## Comandos nodetool bloqueados

O uso dos comandos a seguir é bloqueado pelo nodetool quando aplicados a um serviço {{site.data.keyword.composeForScyllaDB}}:

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
