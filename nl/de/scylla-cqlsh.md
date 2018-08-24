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

# 'cqlsh' verwenden
{: #using-cqlsh}

Die Verbindung zu {{site.data.keyword.composeForScyllaDB_full}} können Sie mit `cqlsh` herstellen.

`cqlsh` ist ein Python 2.7-Programm und kann aufgrund der Schreibweise nicht mit Python 3 ausgeführt werden.
{: .tip}

Da es sich bei `cqlsh` um eine Python-Anwendung handelt, können Sie den Befehl `pip install cqlsh` ausführen, um die neueste Version des Befehls zu installieren.

Sie können `cqlsh` auch aus einer vollständigen Distribution von Cassandra abrufen, die Sie von [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) herunterladen können. Hier werden Binärdaten, Quelle und Anweisungen für die Installation von Cassandra unter ebian (apt) und Red Hat (RPM) Linux-Distributionen angeboten. Laden Sie die Binärdatei eines Release herunter und entpacken Sie die Datei in einem Verzeichnis. Wechseln Sie dann mit `cd` in das Verzeichnis und suchen Sie `cqlsh` im Verzeichnis `bin`.

Wie Sie dieses Tool auf Ihre lokale Einheit abrufen, hängt von Ihrem lokalen Betriebssystem ab. Am einfachsten ist es, das neueste Cassandra-Release mit einer für Ihr Betriebssystem geeigneten Methode zu installieren (die neuesten Versionen unterstützen weiterhin Version 2.1.8, also Scylla) und dann den integrierten `cqlsh`-Befehl zu verwenden. 

Auf einem Mac-Computer können Sie Cassandra zum Beispiel mit `homebrew` installieren, indem Sie `brew install cassandra` eingeben.

## cqlshrc-Dateien

Beim Ausführen des Befehls `cqlsh` wird standardmäßig die Datei `$HOME/.cassandra/cqlshrc` geladen. Anhand dieser Datei können zahlreiche Parameter festgelegt werden, die über die Befehlszeile andernfalls nicht definiert werden können. Durch entsprechendes Bearbeiten der Standarddatei können Sie Parameter festlegen. Alle an der Datei vorgenommenen Änderungen gelten für alle Sitzungen, sofern sie nicht von der der Befehlszeile aus überschrieben werden.

Wenn Sie mehrere Instanzen von Scylla in unterschiedlichen Umgebungen ausführen, können Sie eine separate `cqlshrc`-Datei erstellen und veranlassen, dass diese Datei verwendet wird, indem Sie `--cqlshrc=[filename]` zu Ihrem Befehl hinzufügen.


Ein Beispiel einer vollständigen cqlshrc-Datei enthält das [Apache-Beispiel für cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). 

### cqlshrc für {{site.data.keyword.composeForScyllaDB}}

Eine minimale `cqlshrc`-Datei für die Herstellung einer Verbindung zu Ihrem Service sieht wie folgt aus:

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

Geben Sie die Informationen ein, die von den _Verbindungszeichenfolgen_ des Service bereitgestellt wurden, und speichern Sie die Datei unter dem Namen `example.cqlshrc`.

Sie können sich jetzt bei Ihrer Scylla-Datenbank anmelden, indem Sie den Befehl `cqlsh --ssl --cqlshrc=example.cqlshrc` in der Befehlszeile ausführen. Beachten Sie, dass `cqlshrc` zwar über eine Option zur Angabe von `ssl=true` verfügt, es aber dennoch den Anschein hat, als würde diese Angabe ignoriert, sodass Sie das Flag `--ssl` verwenden müssen.

## TLSv1.2

`cqlsh` nimmt nicht standardmäßig den Wert TLSv1.2 an, wenn eine verschlüsselte Verbindung vereinbart wird. Damit erfolgreich eine Verbindung zu einer Compose Scylla-Umgebung hergestellt werden kann, muss die TLS-Version in der Umgebung angegeben werden, in der die Ausführung von `cqlsh` erfolgt. Um ein Höchstmaß an Kompatibilität zu erzielen und ein gewisses Maß an Zukunftsfähigkeit für TLSv1.3 sicherzustellen, legen Sie in der Umgebung für `SSL_VERSION` den Wert 'SSLv23' fest, indem Sie `export SSL_VERSION=SSLv23` verwenden oder aber dem `cqlsh`-Verbindungsbefehl die Versionsangabe `SSL_VERSION=SSLv23` voranstellen.

Für die Version können Sie auch explizit TLSv1.2 festlegen, indem Sie `export SSL_VERSION=TLSv1_2` verwenden oder dem `cqlsh`-Verbindungsbefehl die Versionsangabe `SSL_VERSION=TLSv1_2` voranstellen.

## Verbindung ohne Validierung

Die Validierung ist standardmäßig aktiviert. Sie können jedoch auch ohne Validieren des fernen Hosts über TLS/SSL eine Verbindung mit Scylla herstellen. Dies wird für Produktionssysteme jedoch nicht empfohlen. Die Validierung wird durch die Umgebungsvariable `SSL_VALIDATE` oder durch SSL-Einstellungen in der cqlshrc-Datei gesteuert. Legen Sie zum Inaktivieren der Validierung in Ihrer Umgebung für `SSL_VALIDATE` den Wert 'false' fest (`export SSL_VALIDATE=false`).

Alternativ können Sie die Datei 'cqlshrc' bearbeiten, um die Validierung zu inaktivieren.

```
[ssl]  
validate = false
```

Wenn Sie die Validierung für einen einzelnen `cqlsh`-Befehl inaktivieren möchten, stellen Sie dem `cqlsh`-Verbindungsbefehl die Angabe `SSL_VALIDATE=false` voran. 

Sie müssen `cqlsh` trotzdem noch mitteilen, dass TLSv1.2 verwendet werden soll. Dies ist auch dann erforderlich, wenn die Validierung nicht aktiviert ist. Wenn Sie die SSL-Version in Ihrer Umgebung nicht festgelegt haben und keine Validierung wünschen, dann sollte die Verbindung wie im folgenden Beispiel aussehen:

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Zertifikat abrufen und verwenden

Wenn Sie cqlsh verwenden wollen und den fernen Host überprüfen möchten, müssen Sie das Zertifikat abrufen, es an einem Ort speichern, auf den Sie Zugriff haben, und dann cqlsh den Pfad zum Zertifikat zur Verfügung stellen.

Legen Sie für die Umgebungsvariable `SSL_CERTFILE` den Pfad und den Dateinamen Ihres gespeicherten Zertifikats fest, um sie für alle nachfolgenden Aufrufe von `cqlsh` zu aktivieren. 

Alternativ können Sie Folgendes zu Ihrer Datei 'cqlshrc' hinzufügen.

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Wenn Sie nur ein bestimmtes Zertifikat für einen einzelnen `cqlsh`-Befehl verwenden möchten, stellen Sie `SSL_CERTFILE = ~/path_to_your/lechain.pem` an den Anfang Ihres 'cqlsh'-Verbindungsbefehls. Beispiel:

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Einführung in cqlsh

Wenn Sie `HELP` eingeben, sehen Sie, dass die Shell eine umfangreiche Funktionalität aufweist. Bei allen Befehlen ist sogar die Befehlszeilenergänzung mittels `TAB`-Vervollständigung möglich. Beispiel:

1. Geben Sie `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>` ein. Nun sollten Sie die Auswahlmöglichkeiten für die Replikationsklasse sehen.
2. Wählen Sie hier die Option `SimpleStrategy` aus, denn der Cluster wird sich nicht über mehrere Rechenzentren erstrecken.
3. Drücken Sie noch einmal `<TAB><TAB>` und geben Sie als Wert für `replication_factor` die Ziffer '3' ein. Schließen Sie dann die geschweifte Klammer mit `}` und schließen Sie die Anweisung mit `;<enter>` ab.

Sie haben nun Ihren ersten Schlüsselbereich (KEYSPACE) erstellt und so konfiguriert, dass Ihre Daten standardmäßig auf allen drei Knoten in Ihrem Cluster repliziert werden.

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

Nun können Sie den Schlüsselbereich verwenden:

```sql 
USE my_new_keyspace;
```

Und Ihre Shell zeigt nun an, dass Ihre Eingabeaufforderung Ihren Schlüsselbereich standardmäßig verwendet. Jede Tabelle muss einen Schlüsselbereich haben.

Geben Sie in der Shell den Befehl `CREATE TABLE` ein, um einen Tabelle zu erstellen, die mit Beispielen gefüllt werden kann.

```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
