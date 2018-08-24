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

# Nodetool verwenden
{: #using-nodetool}

Wenn Sie nodetool zum Verwalten Ihres {{site.data.keyword.composeForScyllaDB_full}}-Service verwenden wollen, müssen Sie zunächst einen SSH-Schlüssel hinzufügen und anschließend mithilfe des SSH-Befehls auf der Registerkarte _Socks_ der Verbindungszeichenfolgen einen Tunnel öffnen.

Wenn der Tunnel offen ist, können Sie `nodetool`-Befehle in dem auf der Registerkarte _Nodetool_ der Verbindungszeichenfolgen gezeigten Format verwenden. Der angegebene Befehl gibt den Status des Scylla-Clusters zurück.

## Verfügbare nodetool-Befehle

Nodetool weist eigentlich eine breite Palette von Befehlen auf. Ihre Anzahl wurde jedoch begrenzt, um Situationen zu vermeiden, die die automatisierte Verwaltung des Clusters gefährden könnten. Die verfügbaren Befehle werden in der Tabelle angezeigt.

Befehl|Funktion
----------|-----------
`cfhistograms`|Stellt Statistikdaten zu Tabellen bereit, wie die Anzahl von SSTables, Latenzzeit für Lesen/Schreiben, Partitionsgröße und Spaltenanzahl.
`cfstats`|Stellt eine umfassende Diagnose zur Spaltenfamilie bereit.
`cleanup`|Löst das sofortige Bereinigen von Schlüsseln aus, die zu keinem Knoten mehr gehören.
`compact`|Erzwingt eine (bedeutende) Komprimierung für mindestens eine Spaltenfamilie.
`compactionhistory`|Gibt den Komprimierungsverlauf aus.
`compactionstats`|Gibt Statistikdaten zu Komprimierungen aus.
`describecluster`|Gibt Name, Snitch, Partitionierungsfunktion und Schemaversion des Clusters aus.
`describering <keyspace>`|Zeigt die Tokenbereichsinformationen eines bestimmten Schlüsselbereichs an.
`flush`|Führt Flushoperationen für mindestens eine Spaltenfamilie aus.
`help`|Gibt Hilfetext aus.
`getendpoints <keyspace> <cfname> <key>`|Gibt die Endpunkte aus, die Eigner des Schlüssels sind.
`gossipinfo`|Zeigt Gossip-Informationen für den Cluster an.
`info`|Gibt Knoteninformationen aus.
`move <new token>`|Verschiebt den Knoten auf dem Token-Ring zu einem neuen Token.
`netstats`|Gibt Netzinformationen zum angegebenen Host aus (standardmäßig der Verbindungsknoten).
`proxyhistograms`|Gibt Statistikhistogramme für Netzoperationen aus.
`repair`|Repariert eine oder mehrere Spaltenfamilie(n).
`ring`|Zeigt Token-Ring-Informationen an.
`status`|Gibt Clusterinformationen aus.
`statusbackup`|Gibt den Status der ScyllaDB-Sicherung aus.
`statusbinary`|Gibt den Status des nativen Transportprotokolls (Binärprotokoll) aus.
`statusgossip`|Gibt den Status von Gossip aus.
`version`|Gibt die DB-Version aus.
{: caption="Tabelle 1. Verfügbare nodetool-Befehle" caption-side="top"}


## Nicht verfügbare nodetool-Befehle

Die folgenden Befehle sind für die Verwendung durch 'nodetool' gesperrt, wenn sie auf einen {{site.data.keyword.composeForScyllaDB}}-Service angewendet werden:

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
