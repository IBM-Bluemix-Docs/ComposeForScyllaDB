---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Verbindungskonfiguration
{: #connection-configuration}

{{site.data.keyword.composeForScyllaDB_full}}-Datenbankverbindungen werden von 3 HAProxy-Portalen verwaltet. Jedes Portal verfügt über 64 MB Hauptspeicher.

Eine Funktionsübernahme auf der Clientseite fällt in die Verantwortung der Anwendungsentwickler. Die meisten Scylla-Treiber sind in dem Sinne clustersensitiv, als dass Sie entweder eine Verbindungszeichenfolge, alle drei Verbindungszeichenfolgen, die Verbindungszuordnungen oder einige beliebige Kombination dieser Angaben bereitstellen können, um Zeichenfolgen für Mehrfachverbindungen und die kontrollierte Funktionsübernahme (Failover) automatisch zur bearbeiten. Solange Sie nicht mit einem Treiber arbeiten, der die Funktionsübernahmefehler vollständig behandelt, sollten Sie Folgendes beachten:

* Arbeiten Sie mit einem Treiber, der mehrere Endpunkte in seiner Verbindungskonfiguration akzeptiert.
* Stellen Sie sicher, dass Ihre Anwendungsaufrufe zum Schreiben in oder Lesen aus der Datenbank angemessen auf Fehler reagieren. Dies schließt folgende Optionen ein:
  + Protokollierung des Fehlers und Wiederherstellen der Verbindung.
  + Wiederherstellen der Verbindung und Wiederholen der Operation, die den Fehler verursacht hat.
  + Steuerung der Warteschlange der fehlgeschlagenen Operation und Planung der Verbindungswiederholung.

## Verschlüsselung in Transit

Alle HAProxy-Portale von {{site.data.keyword.composeForScyllaDB}} sind für TLS/SSL aktiviert und unterstützen TLS Version 1.2. Die Zertifikate für den Service sind Zertifikate von Let's Encrypt. Bestimmte Treiber sowie die Befehlszeilenschnittstelle cqlsh benötigen eine Kopie der Zertifikatskette, um eine Verbindung zu Ihrem Service herstellen zu können. Weitere Informationen finden Sie im Abschnitt zur [Verwendung von Zertifikaten](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html).

## Grenzwerte für die Anzahl von Verbindungen

Für Datenbankverbindungen gilt ein Grenzwert von 1.000 Verbindungen pro Portal. Das Überschreiten des Verbindungsgrenzwerts hat zur Folge, dass Ihr Service nicht mehr reagiert. Über die Funktion Ihres Treibers für Verbindungspooling können Sie Verbindungen von ihrer Anwendung aus verwalten.

## Proxy-Zeitlimits

Die HAProxy-Portale verwalten den Lebenszyklus von Verbindungen zwischen dem Server und dem Client. Die Zeitlimitwerte sind hier als Orientierung für Treiberautoren und all diejenigen aufgeführt, die sich für die maschinennahe Aktivität von {{site.data.keyword.composeForScyllaDB}}-Datenbankverbindungen interessieren.

Einstellung | Wert | Gültigkeit
----------|-----------|-----------
client | 5 m | Wenn vom Client erwartet wird, Daten zu bestätigen oder zu senden.
connect | 4 s | Wenn eine Verbindung zwischen dem Proxy und dem Server hergestellt wird.
server | 5 m | Wenn vom Server erwartet wird, Daten zu bestätigen oder zu senden.
tunnel | 1 Tag | Wenn eine bidirektionale Verbindung zwischen einem Client und einem Server hergestellt wurde und die Verbindung in beiden Richtungen inaktiv bleibt.
client-fin | 1 h | Wenn vom Client erwartet wird, Daten zu bestätigen oder zu senden, während eine Verbindungsrichtung bereits beendet wurde.
check | 5 s | Als sekundäre Zeitlimitüberprüfung, wenn eine Verbindung zwischen dem Proxy und dem Server hergestellt wird.

{: caption="Tabelle 1. HAProxy-Zeitlimitwerte für Scylla" caption-side="top"}
