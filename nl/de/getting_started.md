---

copyright:
  years: 2016,2018
lastupdated: "2018-02-16"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Lernprogramm 'Einführung'
Dieses Lernprogramm verwendet die Beispielapp [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs), um die Verwendung von Node.js zur Herstellung einer Verbindung zu einem {{site.data.keyword.composeForScyllaDB_full}}-Service mithilfe der bereitgestellten Berechtigungsnachweise zu veranschaulichen. Die Anwendung erstellt eine Datenbank, liest daraus und schreibt darin. Dabei nutzt sie Daten, die über die Webschnittstelle der App bereitgestellt werden.
{: shortdesc}

## Vorbereitende Schritte

Stellen Sie sicher, dass Sie über ein [{{site.data.keyword.cloud_notm}}-Konto][ibm_cloud_signup_url]{:new_window} verfügen.

Sie müssen [Node.js](https://nodejs.org/) und [Git](https://git-scm.com/downloads) installieren.

## Schritt 1: {{site.data.keyword.composeForScyllaDB}}-Serviceinstanz erstellen
{: #create-service}

Sie können einen {{site.data.keyword.composeForScyllaDB}}-Service über die [{{site.data.keyword.composeForScyllaDB}}-Seite](https://console.{DomainName}/catalog/services/compose-for-scylladb/) im {{site.data.keyword.cloud_notm}}-Katalog erstellen.

Wählen Sie einen Servicenamen, eine Region, eine Organisation und einen Bereich zur Bereitstellung des Service aus und wählen Sie für das Feld **Datenbankversion auswählen** die _Letzte bevorzugte Version_.

Wählen Sie dann einen Preistarif für Ihren Service aus. Sie können den Plan *Standard* oder *Enterprise* auswählen. Mit dem Plan *Enterprise* können Sie Ihre {{site.data.keyword.composeForScyllaDB}}-Instanz in einem {{site.data.keyword.composeEnterprise}}-Cluster bereitstellen. {{site.data.keyword.composeEnterprise}} stellt die für die Konformität mit Enterprise erforderliche Sicherheit und Isolation bereit und stellt mithilfe eines dedizierten Netzbetriebs die Leistung der bereitgestellten Datenbanken sicher. Weitere Details finden Sie in der Dokumentation zu [Compose Enterprise](../ComposeEnterprise/index.html).

## Schritt 2: Beispielapp 'Hello World' aus Github klonen

Klonen Sie die App 'Hello World' mit folgendem Befehl von Ihrem Terminal auf Ihre lokale Umgebung:

```
git clone https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs.git
```

## Schritt 3: App-Abhängigkeiten installieren

Verwenden Sie npm zum Installieren der Abhängigkeiten.

1. Wechseln Sie von Ihrem Terminal in das Verzeichnis, in dem sich die Beispielapp befindet.
  
  ```
  cd compose-scylladb-helloworld-nodejs
  ```

2. Installieren Sie die in der Datei `package.json` aufgelisteten Abhängigkeiten.
  
  ```
  npm install
  ```

## Schritt 4: Serviceberechtigungsnachweise erstellen

Bevor Sie die App mit einer Push-Operation an {{site.data.keyword.cloud_notm}} übertragen, können Sie sie lokal ausführen, um die Verbindung zu Ihrer {{site.data.keyword.composeForScyllaDB}}-Serviceinstanz zu testen. Um die Verbindung zum Service herzustellen, müssen Sie eine Reihe von Serviceberechtigungsnachweisen erstellen.

1. Öffnen Sie von Ihrem {{site.data.keyword.cloud_notm}}-Dashboard aus Ihre {{site.data.keyword.composeForScyllaDB}}-Serviceinstanz.
2. Wählen Sie _Serviceberechtigungsnachweise_ im Hauptmenü aus, um die Ansicht mit den Serviceberechtigungsnachweisen zu öffnen.
3. Klicken Sie auf **Neuer Berechtigungsnachweis**.
4. Wählen Sie einen Namen für Ihre Berechtigungsnachweise aus und klicken Sie auf **Hinzufügen**.
5. Ihre neuen Berechtigungsnachweise werden nun aufgelistet. Klicken Sie auf **Berechtigungsnachweise anzeigen** in der entsprechenden Zeile der Tabelle, um die Berechtigungsnachweise anzuzeigen, und klicken Sie auf das Symbol **Kopieren**, um Ihre Berechtigungsnachweise zu kopieren.
6. Erstellen Sie in Ihrem bevorzugten Editor eine neue Datei mit folgenden Angaben und fügen Sie Ihre Berechtigungsnachweise wie gezeigt ein:

  ```
  {
    "services": {
      "compose-for-scylladb": [
        {
          "credentials": BERECHTIGUNGSNACHWEISE HIER EINFÜGEN
        }
      ]
    }
  }
  ```
6. Speichern Sie die Datei als `vcap-local.json` in dem Verzeichnis, in dem sich die Beispielapp befindet.

Damit Sie Ihre Berechtigungsnachweise nicht unbeabsichtigt zugänglich machen, wenn Sie die Anwendung mit einer Push-Operation an Github oder {{site.data.keyword.cloud_notm}} übertragen, sollten Sie sicherstellen, dass die Datei, die Ihre Berechtigungsnachweise enthält, in der entsprechenden ignore-Datei aufgelistet ist. Wenn Sie `.cfignore` und `.gitignore` in Ihrem Anwendungsverzeichnis öffnen, sehen Sie, dass `vcap-local.json` in beiden Dateien aufgelistet ist. Das heißt, sie gehören nicht zu den Dateien, die bei der Push-Übertragung der App an Github oder {{site.data.keyword.cloud_notm}} hochgeladen werden.
{: .tip}

## Schritt 5: App lokal ausführen

```
npm start
```

Die App wird jetzt unter [http://localhost:8080](http://localhost:8080) ausgeführt. Sie können Ihrer {{site.data.keyword.composeForScyllaDB}}-Datenbank Wörter und Definitionen hinzufügen. Wenn Sie die App stoppen und erneut starten, werden alle bereits hinzugefügten Wörter angezeigt, sobald Sie die Seite aktualisieren.

Als Nächstes muss Ihre App mit Ihrer Serviceinstanz verbunden und in {{site.data.keyword.cloud_notm}} bereitgestellt werden.

## Schritt 6: {{site.data.keyword.cloud_notm}}-CLI-Tool herunterladen und installieren

Sie verwenden das {{site.data.keyword.cloud_notm}}-CLI-Tool für die Kommunikation mit {{site.data.keyword.cloud_notm}} über Ihr Terminal oder Ihre Befehlszeile. Details hierzu finden Sie unter [{{site.data.keyword.cloud_notm}}-CLI herunterladen und installieren](https://console.{DomainName}/docs/cli/reference/bluemix_cli/download_cli.html).

## Schritt 7: Verbindung zu {{site.data.keyword.cloud_notm}} herstellen

1. Stellen Sie im Befehlszeilentool eine Verbindung zu {{site.data.keyword.cloud_notm}} her und befolgen Sie die Eingabeaufforderungen zur Anmeldung.

  ```
  bx login
  ```

  Wenn Sie eine eingebundene Benutzer-ID haben, verwenden Sie den Befehl `bx login --sso` für die Anmeldung mit Ihrer SSO-ID (Single Sign On). Weitere Informationen finden Sie unter [Mit eingebundener ID anmelden](https://console.{DomainName}/docs/cli/login_federated_id.html#federated_id).
  {: .tip}

2. Stellen Sie sicher, dass Sie als Ziel die richtige Organisation und den richtigen Bereich von {{site.data.keyword.cloud_notm}} verwenden.

  ```
  bx target --cf
  ```

  Wählen Sie Werte aus den verfügbaren Optionen aus. Verwenden Sie dabei dieselben Werte, die Sie auch zum Erstellen des Service verwendet haben.

## Schritt 8: Manifestdatei der App aktualisieren
{: #update-manifest}

{{site.data.keyword.cloud_notm}} verwendet eine Manifestdatei - `manifest.yml`, um eine Anwendung einem Service zuzuordnen. Befolgen Sie diese Schritte, um Ihre Manifestdatei zu erstellen.

1. Öffnen Sie in einem Editor eine neue Datei und fügen Sie folgende Angaben hinzu:

  ```
  ---
  applications:
  - name:    compose-scylladb-helloworld-nodejs
    host:    compose-scylladb-helloworld-nodejs
    memory:  128M
    services:
      - my-compose-for-scylladb-service
  ```

2. Ändern Sie den Wert für `host` in einen eindeutigen Wert. Der gewählte Host bestimmt die Unterdomäne Ihrer Anwendungs-URL: `<host>.mybluemix.net`.
3. Ändern Sie den Wert für `name`. Der gewählte Wert wird als Name der App in Ihrem {{site.data.keyword.cloud_notm}}-Dashboard angezeigt.
4. Aktualisieren Sie den Wert für `services`, sodass er mit dem Namen des Service übereinstimmt, den Sie im Schritt [{{site.data.keyword.composeForScyllaDB}}-Serviceinstanz erstellen](#create-service) erstellt haben. 

## Schritt 9: App mit einer Push-Operation an {{site.data.keyword.cloud_notm}} übertragen

Wenn Sie die App mit einer Push-Operation übertragen, wird sie automatisch an den Service gebunden, der in der Manifestdatei angegeben ist.

```
bx cf push
```

## Schritt 10: Prüfen, ob die App mit Ihrem {{site.data.keyword.composeForScyllaDB}}-Service verbunden ist

1. Navigieren Sie zum {{site.data.keyword.composeForScyllaDB}}-Service-Dashboard
2. Wählen Sie _Verbindungen_ im Dashboard-Menü aus. Ihre Anwendung sollte unter _Verbundene Anwendungen_ aufgelistet sein.

Ist Ihre Anwendung nicht aufgelistet, wiederholen Sie die Schritte 7 und 8 und prüfen Sie dabei, ob Sie die richtigen Angaben in die Datei [manifest.yml](#update-manifest) eingegeben haben.

## Schritt 11: App verwenden

Wenn Sie nun `<host>.mybluemix.net/` besuchen, können Sie den Inhalt Ihrer {{site.data.keyword.composeForScyllaDB}}-Sammlung sehen. Während Sie Wörter und die zugehörigen Definitionen hinzufügen, werden sie in die Datenbank aufgenommen und angezeigt. Wenn Sie die App stoppen und erneut starten, werden alle Wörter und Definitionen, die Sie bereits hinzugefügt haben, aufgelistet.


## Nächste Schritte

Zum besseren Verständnis der Beispielapp [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) können Sie die Readme-Datei der Anwendung lesen, oder die Codekommentare in `server.js`, die einige Informationen zu den Funktionen der App enthalten.

Die folgenden Themen zum Service-Dashboard geben Ihnen einen ersten Einstieg in Ihren {{site.data.keyword.composeForScyllaDB}}-Service:

- [Übersicht über das Dashboard](./dashboard-overview.html)
- [Sicherungen](./dashboard-backups.html)
- [Einstellungen](./dashboard-settings.html)

Informationen zu den von Ihnen erstellten Berechtigungsnachweisen für die Verbindung zwischen der Anwendung und Ihrem Service finden Sie im Abschnitt [Verfügbare Berechtigungsnachweise](./connecting-bluemix-app.html#available-credentials).

[ibm_cloud_signup_url]: https://ibm.biz/compose-for-scylladb-signup
