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

# Architektur 
{: #architecture}

## Replikation

Ein {{site.data.keyword.composeForScyllaDB_full}}-Service enthält einen Cluster mit drei Datenreplikaten. Alle Replikate sind gleichermaßen wichtig. Es gibt kein Primär- oder Masterreplikat und jeder Knoten kennt den Status der jeweils anderen Knoten. Sie wählen eine Region aus, in der der Service bereitgestellt wird, und die drei Datenreplikate werden über die Verfügbarkeitszonen der Region verteilt. Anschließend werden die Daten in alle drei Verfügbarkeitszonen kopiert. Sollte ein Datenelement (Member) nicht mehr erreichbar sein, setzt Ihr Cluster seinen Betrieb ordnungsgemäß fort.

## Plattenverschlüsselung

Für alle {{site.data.keyword.composeForScyllaDB}}-Services erfolgt eine Verschlüsselung im Ruhezustand. Ihre Daten befinden sich auf Servern, auf denen die Verschlüsselung auf Datenträgerebene aktiviert ist. 

Da es sich bei {{site.data.keyword.composeForScyllaDB}} um einen verwalteten Service handelt, sind Operatoren von Compose auf {{site.data.keyword.cloud_notm}} in der Lage, Daten zu sehen. Zusätzlich zur Plattenverschlüsselung sollten Sie persönliche Informationen verschlüsseln, bevor diese in der Datenbank gespeichert werden, oder Sie sollten Erweiterungen oder Features verwenden, um die Verschlüsselung auf der Datenbank selbst zu ermöglichen. Obwohl diese Methoden den Bedienungskomfort oder die Leistung beeinträchtigen können, hat es sich bewährt, den Schutz persönlicher Informationen mit Verschlüsselung sicherzustellen.

## Automatische Skalierung

Die Ressourcen werden basierend auf der Gesamtplattenspeicherbelegung der Scylla-Datenbanken automatisch skaliert. Der Hauptspeicher basiert auf einem Faktor des bereitgestellten Plattenspeichers im Verhältnis von 1:10. Diese Ressourcen werden als Einheiten gebündelt.

Die automatische Skalierung ist so konzipiert, dass sie als Reaktion auf die kurz-bis mittelfristigen Trends Ihrer Datenbank erfolgt. Ihr Service wird in stündlichen Intervallen überprüft. Wenn der Plattenspeicherplatz für den Service knapp zu drohen wird, werden der Bereitstellung weitere Einheiten zugeordnet. 

Bei der automatischen Skalierung wird kein Scale-down für Bereitstellungen durchgeführt, bei denen sich die Platten-/Speicherbelegung verringert hat. Die für Ihre Datenbanken bereitgestellten Ressourcen bleiben für künftige Anforderungen weiterhin bestehen, sofern Sie Ihre Bereitstellung nicht per Scale-down manuell nach unten skalieren.
{: .tip}


