---

copyright:
  years: 2016,2018
lastupdated: "2018-01-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Prezzi
{: #pricing}

## Configurazione di base
Un servizio {{site.data.keyword.composeForScyllaDB_full}} viene fornito con 3 nodi membro, ognuno con 5GB di archiviazione e 512MB di memoria, che equivale a 5 unità di risorse. Il servizio _include_ la replica e l'elevata disponibilità, per cui ogni unità e il prezzo per unità _include_ il costo delle risorse in tutti e tre i nodi.

La configurazione di base inoltre include 3 portali HAProxy che forniscono le connessioni al cluster. Ogni portale HAProxy ha 64MB e supporta l'autenticazione, HTTPS e la whitelist di IP.

### Costo
La configurazione del servizio di base a un prezzo fissato. Consulta i tile del catalogo {{site.data.keyword.cloud_notm}} per i prezzi di base nella tua valuta locale. Ad esempio, il prezzo di base in dollari US è $90/mese.

## Opzioni di espansione
Se hai bisogno di ulteriore archiviazione o memoria per il tuo servizio, puoi aumentare le risorse assegnate in un rapporto 10:1 di archiviazione del disco per unità di memoria. L'aumento del disco assegnato alla distribuzione aumenterà anche la RAM assegnata. Un'unità {{site.data.keyword.composeForScyllaDB}} è composta da 1GB di archiviazione e 102MB di memoria e ogni unità e prezzo per unità _include_ il costo di incremento delle risorse su tutti e tre i nodi del cluster. 

### Costo
Ogni unità aggiuntiva (1GB di archiviazione/102MB di memoria) ha un prezzo per unità, che viene elencato nella tua valuta corrente nel tile del catalogo {{site.data.keyword.cloud_notm}} per il servizio. In dollari US ogni unità aggiuntiva costa $18. Quando la dimensione _totale_ di tutti i tuoi servizi {{site.data.keyword.composeForScyllaDB}} aumenta, il prezzo per unità diminuisce, come mostrato nella seguente tabella dei prezzi a livelli.

### Prezzi a livelli
Numero di unità|Prezzo per unità
----------|-----------
5 - 9 unità|prezzo per unità di base -- $18.00 USD/Unità
10 - 24 unità|10% di riduzione -- $16.20 USD/Unità
25 - 49 unità|20% di riduzione -- $14.40 USD/Unità
50 - 99 unità|30% di riduzione -- $12.60 USD/Unità
100 - 499 unità|40% di riduzione --$10.80 USD/Unità
500 - 999 unità|50% di riduzione -- $9.00 USD/Unità
1.000 - 4.999 unità|60% di riduzione -- $7.20 USD/Unità
5.000+ unità|70% di riduzione -- $5.40 USD/Unità
{: caption="Tabella 1. Prezzi a livelli di {{site.data.keyword.composeForScyllaDB}}" caption-side="top"}
