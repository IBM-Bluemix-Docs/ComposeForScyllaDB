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

# Utilizzo di nodetool
{: #using-nodetool}

Per utilizzare nodetool per gestire il tuo servizio {{site.data.keyword.composeForScyllaDB_full}}, devi prima aggiungere una chiave SSH e poi aprire un tunnel utilizzando il comando SSH nella scheda _Socks_ delle stringhe di connessione.

Quando viene aperto un tunnel, puoi utilizzare i comandi `nodetool` con il formato mostrato nella scheda _Nodetool_ delle stringhe di connessione. Il comando fornito restituisce lo stato del cluster Scylla.

## Comandi nodetool disponibili

Nodetool ha una vasta gamma di comandi, ma il loro utilizzo è limitato per evitare situazioni che potrebbero danneggiare la manutenzione automatizzata del cluster. I comandi disponibili vengono visualizzati nella tabella.

Comando|Funzione
----------|-----------
`cfhistograms`|Fornisce le statistiche di una tabella, inclusi il numero di SSTables, la latenza scrittura/lettura, la dimensione della partizione e il numero di colonne.
`cfstats`|Fornisce diagnosi approfondite sulla famiglia di colonne.
`cleanup`|Attiva la ripulitura immediata delle chiavi che non appartengono più a un nodo.
`compact`|Forza una compressione (maggiore) di una o più famiglie di colonne.
`compactionhistory`|Stampa la cronologia di compressione.
`compactionstats`|Stampa le statistiche sulle compressioni.
`describecluster`|Stampa il nome, lo snitch, il partitioner e la versione dello schema di un cluster.
`describering <keyspace>`|Mostra le informazioni sugli intervalli di token per un keyspace.
`flush`|Elimina una o più famiglie di colonne.
`help`|Stampa la guida.
`getendpoints <keyspace> <cfname> <key>`|Stampa gli endpoint che gestiscono la chiave.
`gossipinfo`|Mostra le informazioni di gossip di un cluster.
`info`|Stampa le informazioni sul nodo.
`move <new token>`|Sposta il nodo sul token ring in un nuovo token.
`netstats`|Stampa le informazioni della rete sull'host fornito (connessione del nodo per impostazione predefinita).
`proxyhistograms`|Stampa gli istogrammi delle statistiche per le operazioni di rete.
`repair`|Ripristina una o più famiglie di colonne.
`ring`|Visualizza le informazioni sul token ring.
`status`|Stampa le informazioni sul cluster.
`statusbackup`|Stampa lo stato del backup ScyllaDB.
`statusbinary`|Stampa lo stato del trasporto nativo (protocollo binario).
`statusgossip`|Stampa lo stato del gossip.
`version`|Stampa la versione DB.
{: caption="Tabella 1. Comandi nodetool disponibili" caption-side="top"}


## Comandi nodetool bloccati

I seguenti comandi sono bloccati dall'utilizzo di nodetool quando applicato a un servizio {{site.data.keyword.composeForScyllaDB}}:

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
