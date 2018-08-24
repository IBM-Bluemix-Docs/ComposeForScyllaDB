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

# Utilización de nodetool
{: #using-nodetool}

Para utilizar nodetool para administrar el servicio de {{site.data.keyword.composeForScyllaDB_full}}, primero tiene que añadir una clave SSH y, a continuación, abrir un túnel utilizando el mandato SSH en el separador _Socks_ de las series de conexión.

Cuando el túnel esté abierto, puede utilizar los mandatos `nodetool` con el formato mostrado en el separador _Nodetool_ de las series de conexión. El mandato proporcionado devuelve el estado del clúster Scylla.

## Mandatos nodetool disponibles

Nodetool tiene una amplia gama de mandatos, pero su uso está limitado para evitar situaciones que podrían dañar el mantenimiento automático del clúster. Los mandatos disponibles se muestran en la tabla.

Mandato|Función
----------|-----------
`cfhistograms`|Proporcionar estadísticas sobre una tabla, incluido el número de SSTables, latencia de lectura/escritura, tamaño de partición y recuento de columnas.
`cfstats`|Proporcionar diagnóstico en profundidad en relación con la familia de columnas.
`cleanup`|Desencadenar la limpieza de claves inmediata que ya no pertenece a un nodo
`compact`|Forzar una compactación (mayor) en una o varias familias de columnas.
`compactionhistory`|Imprimir el historial de compactación.
`compactionstats`|Imprimir estadísticas de compactación.
`describecluster`|Imprimir el nombre, el chivato, el particionador y la versión del esquema de un clúster.
`describering <keyspace>`|Mostrar la información de rangos de señales de un espacio de claves.
`flush`|Desechar una o varias familias de columnas.
`help`|Imprimir la ayuda.
`getendpoints <keyspace> <cfname> <key>`|Imprimir los puntos finales propietarios de la clave.
`gossipinfo`|Mostrar la información de rumores ("gossip information") para el clúster.
`info`|Imprimir información del nodo.
`move <new token>`|Mover el nodo de la red en anillo a una nueva red.
`netstats`|Imprimir la información de red en el host proporcionado (conectar el nodo de forma predeterminada).
`proxyhistograms`|Imprimir los histogramas de estadísticas para las operaciones de red.
`repair`|Reparar una o varias familias de columnas.
`ring`|Mostrar la información de red en anillo.
`status`|Imprimir información del clúster.
`statusbackup`|Imprimir el estado de copia de seguridad de ScyllaDB.
`statusbinary`|Imprimir estado del transporte nativo (protocolo binario).
`statusgossip`|Imprimir estado de rumores.
`version`|Imprimir la versión de la base de datos.
{: caption="Tabla 1. Mandatos nodetool disponibles" caption-side="top"}


## Mandatos nodetool bloqueados

Los mandatos siguientes están bloqueados de su uso por nodetool cuando se aplican a un servicio de {{site.data.keyword.composeForScyllaDB}}:

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
