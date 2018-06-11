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

# 'cqlsh' verwenden
{: #using-cqlsh}

Sie können mit `cqlsh` eine Direktverbindung zu {{site.data.keyword.composeForScyllaDB_full}} herstellen.
Es gibt eine Reihe von Möglichkeiten der Installation. Da es sich um eine Python-Anwendung handelt, führt die Ausführung des Befehls `pip install cqlsh` zur Installation der letzten Version des Befehls. `cqlsh` ist ein Python 2.7-Programm und kann aufgrund der Schreibweise nicht mit Python 3 ausgeführt werden.

Es ist ferner möglich, `cqlsh` von einer vollständigen Cassandra-Distribution abzurufen. Diese kann von [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) abgerufen werden. Hier werden Binärdateien, Quelle und Installationsanweisungen für Cassandra unter Debian (apt) und Red Hat (RPM) Linux-Distributionen angeboten. Laden Sie die Binärdatei eines Release herunter und entpacken Sie die Datei in einem Verzeichnis. Wechseln Sie dann mit `cd` in das Verzeichnis und suchen Sie `cqlsh` im Verzeichnis `bin`. 

Wie Sie dieses Tool auf Ihre lokale Einheit abrufen, hängt von Ihrer lokalen Plattform ab. Am einfachsten ist es, das neueste Cassandra-Release mit einer für Ihre Plattform geeigneten Methode zu installieren (die neuesten Versionen unterstützen weiterhin Version 2.1.8, also Scylla) und den integrierten Befehl `cqlsh` zu verwenden. 

Installieren Sie auf einem Mac zum Beispiel Cassandra mit `homebrew`, indem Sie Folgendes eingeben: `brew install cassandra`.

## Informationen zu 'cqlshrc'-Dateien
Bei der Ausführung des Befehls `cqlsh` wird standardmäßig die Datei `$HOME/.cassandra/cqlshrc` geladen, in der zahlreiche Parameter definiert werden können, die nicht über die Befehlszeile definierbar sind. Ein [Apache-Beispiel für 'cqlshrc'](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). Sie können die Standarddatei bearbeiten und Parameter festlegen. Dies gilt für alle Sitzungen, bis diese von der Befehlszeile überschrieben werden. 

Wenn Sie mehrere Instanzen von Scylla in unterschiedlichen Umgebungen ausführen, können Sie eine separate `cqlshrc`-Datei erstellen und veranlassen, dass `cqlsh` diese Datei verwendet, indem Sie `--cqlshrc=[filename]` zu Ihrem Befehl hinzufügen. 

### 'cqlshrc' für {{site.data.keyword.composeForScyllaDB}}
Dies ist ein Beispiel für eine minimale `cqlshrc`-Datei für die Verbindung zu Ihrem Service:
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

Geben Sie die Informationen ein, die von den _Verbindungszeichenfolgen_ des Service bereitgestellt wurden und speichern Sie diese Datei als 'example.cqlshrc'.
Durch die Ausführung von `cqlsh --ssl --cqlshrc=example.cqlshrc` von der Befehlszeile werden Sie bei Ihrer Scylla-Datenbank angemeldet. Beachten Sie, dass `cqlshrc` zwar über eine Option zur Angabe von `ssl=true` verfügt, es aber dennoch den Anschein hat, als würde dies ignoriert, so dass das Flag `--ssl` verwendet werden muss. 

Die SSL-Version kann nicht in der Datei `cqlshrc` festgelegt werden, sodass Sie entweder für die Umgebung die Verwendung von TLSv1.2 festlegen müssen oder SSL_VERSION=TLSv1_2 an den Anfang des Verbindungsbefehls `cqlsh` stellen müssen. 

## TLSv1.2

`cqlsh` nimmt nicht standardmäßig den Wert TLSv1.2 an, wenn eine verschlüsselte Verbindung vereinbart wird. Um erfolgreich eine Verbindung zu einer Compose Scylla-Umgebung herzustellen, muss die TLS-Version in der Umgebung angegeben werden, in der `cqlsh` ausgeführt wird. Legen Sie SSL_VERSION in der Umgebung wie folgt fest:
`export SSL_VERSION=TLSv1_2`
oder stellen Sie `SSL_VERSION=TLSv1_2` im Verbindungsbefehl vor `cqlsh`. 

## Verbindung ohne Validierung

Die einfachste Art, eine Verbindung zu Scylla mit TLS/SSL herzustellen, erfolgt ohne Validierung des fernen Hosts. Dies wird für die Produktion nicht empfohlen. Standardmäßig ist die Validierung ausgewählt. Die Validierung wird durch die Umgebungsvariable `SSL_VALIDATE` oder durch die SSL-Einstellungen in der Datei 'cqlshrc' gesteuert. Legen Sie für `SSL_VALIDATE` in Ihrer Umgebung den Wert 'false' fest, um die Validierung zu inaktivieren. `export SSL_VALIDATE=false`

Alternativ können Sie Folgendes zu Ihrer Datei 'cqlshrc' hinzufügen. 

```
[ssl]  
validate = false
```

Wenn Sie die Validierung nur für einen einzelnen `cqlsh`-Befehl inaktivieren möchten, stellen Sie `SSL_VALIDATE=false` vor den `cqlsh`-Verbindungsbefehl.  

Sie müssen `cqlsh` immer noch mitteilen, dass TLSv1.2 verwendet werden soll. Dies ist auch dann erforderlich, wenn das Zertifikat nicht validiert wird. Wenn Sie die SSL-Version in Ihrer Umgebung nicht festgelegt haben und keine Validierung wünschen, dann sollte die Verbindung wie im folgenden Beispiel aussehen: 

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Zertifikat abrufen und verwenden

Wenn Sie 'cqlsh' verwenden und den fernen Host überprüfen möchten, müssen Sie das Zertifikat abrufen, es an einem Ort speichern, auf den Sie Zugriff haben, und dann den Pfad zum Zertifikat 'cqlsh' zur Verfügung stellen. Eine Kopie des Zertifikats befindet sich ebenso wie Details zu seinem Inhalt und dessen Quellen auf der Seite [Scylla und Zertifikate](doc:scylla-and-certificates). Erstellen Sie eine Zertifikatsdatei oder speichern Sie eine Kopie lokal.  

Legen Sie für die Umgebungsvariable `SSL_CERTFILE` den Pfad und den Dateinamen Ihres gespeicherten Zertifikats fest, um sie für nachfolgende Aufrufe von `cqlsh` zu aktivieren. 

Alternativ können Sie Folgendes zu Ihrer Datei 'cqlshrc' hinzufügen. 
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Wenn Sie nur ein bestimmtes Zertifikat für einen einzelnen `cqlsh`-Befehl verwenden möchten, stellen Sie `SSL_CERTFILE = ~/path_to_your/lechain.pem` an den Anfang Ihres 'cqlsh'-Verbindungsbefehls. Beispiel:

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## 'cqlsh' verwenden

Wenn Sie `HELP` eingeben, sehen Sie, dass die Shell eine umfangreiche Funktionalität aufweist. Noch praktischer ist es, dass alle diese Befehle auch eine Fertigstellung namens `TAB` besitzen. Beispiel:
1. Geben Sie `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>` ein. Nun sollten Sie die Auswahlmöglichkeiten für die Replikationsklasse sehen.
2. Sie können hier `SimpleStrategy` auswählen, weil sich der Cluster nicht über mehrere Rechenzentren erstreckt.
3. Drücken Sie noch einmal `<TAB><TAB>` und geben Sie '3' für 'replication_factor' ein. Schließen Sie dann die geschweifte Klammer mit `}` und schließen Sie die Anweisung mit `;<enter>` ab.

Sie haben gerade Ihren ersten Schlüsselbereich (KEYSPACE) erstellt und so eingestellt, dass Ihre Daten standardmäßig auf alle drei Knoten in Ihrem Cluster repliziert werden.
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
