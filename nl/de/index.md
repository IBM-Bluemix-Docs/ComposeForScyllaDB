---

copyright:
  years: 2016,2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung in Compose for ScyllaDB
{: #getting-started-with-compose-for-scylladb}

ScyllaDB ist ursprünglich ein Ersatz für die verteilte Wide Column-Cassandra-Datenbank. ScyllaDB ist für eine bessere Ressourcennutzung, die zu einer zehnmal besseren Benchmarkleistung führen kann, in C++ geschrieben, nicht wie Cassandra in Java. ScyllaDB behält nicht nur die Kompatibilität mit Cassandra-Tools und Datendateien bei, sondern fügt auch Funktionen zur automatischen Leistungsoptimierung hinzu. Mithilfe von {{site.data.keyword.composeForScyllaDB_full}} werden die Funktionen von ScyllaDB dahingehend erweitert, dass die Verwaltung für Sie übernommen wird und Sie ein einfaches Bereitstellungssystem zur automatischen Skalierung für eine hohe Verfügbarkeit und Redundanz sowie automatisierte Backups angeboten bekommen.
{:shortdesc}

**Anmerkung:** {{site.data.keyword.composeForScyllaDB_full}} gewährt keinen Zugriff auf die Compose-Benutzerschnittstelle. Weitere Details finden Sie unter [Unterstützung für Compose on {{site.data.keyword.cloud}}](https://help.compose.com/docs/bluemix-compose-support).

## Compose for ScyllaDB-Serviceinstanz erstellen

[Erstellen Sie eine {{site.data.keyword.composeForScyllaDB}}-Instanz](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/).

Wenn Sie eine Instanz des Service erstellen, wählen Sie sowohl einen Namen für den Service als auch einen Namen für den Berechtigungsnachweis aus. Lassen Sie den Service ungebunden; Sie können später eine Anwendung mit Ihrem Service verbinden, indem Sie die Berechtigungsnachweise verwenden, die bei der Bereitstellung des Service angegeben wurden. Die verschiedenen Werte für die Berechtigungsnachweise sind im Abschnitt "Verfügbare Berechtigungsnachweise" aufgeführt.

Bei der Bereitstellung Ihrer {{site.data.keyword.composeForScyllaDB}}-Instanz können Sie den Plan *Standard* oder *Enterprise* auswählen. Mit dem Plan *Enterprise* können Sie Ihre {{site.data.keyword.composeForScyllaDB}}-Instanz in einem {{site.data.keyword.composeEnterprise}}-Cluster bereitstellen. {{site.data.keyword.composeEnterprise}} stellt die für die Konformität mit Enterprise erforderliche Sicherheit und Isolation bereit und stellt mithilfe eines dedizierten Netzbetriebs die Leistung der bereitgestellten Datenbanken sicher. Weitere Details finden Sie in der [Dokumentation zu Compose Enterprise](../ComposeEnterprise/index.html).

## Compose for ScyllaDB verwalten

Sie können Ihren Service über das Service-Dashboard verwalten. Hier finden Sie Informationen zu Ihrer {{site.data.keyword.cloud_notm}} Compose-Datenbank und dazu, wie Sie eine Verbindung zu ihr herstellen. Außerdem können Sie:

- Ihre Sicherungen verwalten
- Ihrem Service mehr Ressourcen zuordnen 
- Whitelists verwenden, um den Zugriff auf Ihre Datenbank zu beschränken. 

Weitere Informationen finden Sie im Abschnitt [Einstellungen](./dashboard-settings.html).

## Verbindung zu Compose for ScyllaDB herstellen

Sie können mit den Berechtigungsnachweisen, die zusammen mit Ihrem Service erstellt werden, oder mithilfe der Verbindungszeichenfolgen und der Befehlszeile, die auf der Registerkarte *Übersicht* Ihres Service-Dashboards bereitgestellt werden, eine Verbindung zu dem Service herstellen.

## Verbindung von einer {{site.data.keyword.cloud_notm}}-Anwendung zu Compose for ScyllaDB herstellen

Um eine {{site.data.keyword.cloud_notm}}-Anwendung mit Ihrem Service zu verbinden, verwenden Sie die Berechtigungsnachweise, die zusammen mit dem Service erstellt wurden. Informationen zum Herstellen einer Verbindung von einer {{site.data.keyword.cloud_notm}}-Anwendung zu einem {{site.data.keyword.composeForScyllaDB}}-Service finden Sie im Abschnitt [{{site.data.keyword.cloud_notm}}-Anwendung verbinden](./connecting-bluemix-app.html).

## Verbindung zu Compose for ScyllaDB außerhalb von {{site.data.keyword.cloud_notm}} herstellen

Wenn Sie außerhalb von {{site.data.keyword.cloud_notm}} eine Verbindung zu {{site.data.keyword.composeForScyllaDB}} herstellen wollen, können Sie dazu die bereitgestellten Verbindungszeichenfolgen oder die Befehlszeile verwenden. Informationen dazu, wie sich eine Verbindung herstellen lässt, finden Sie im Abschnitt [Externe Anwendung verbinden](./connecting-external.html).
