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

# Arquitectura 
{: #architecture}

## Réplica

Un servicio de {{site.data.keyword.composeForScyllaDB_full}} contiene un clúster con tres réplicas de datos. Todas las réplicas son igualmente importantes; no hay ninguna réplica primaria ni maestra, y cada nodo conoce el estado de los otros nodos. Seleccione una región en la que se ha desplegado el servicio, las tres réplicas de datos se distribuirán a través de las zonas de disponibilidad de la región y, a continuación, los datos se copiarán en los tres. Si un miembro de datos se vuelve inalcanzable, el clúster continúa operando con normalidad.

## Cifrado de disco

Todos los servicios de {{site.data.keyword.composeForScyllaDB}} tienen cifrado en reposo. Los datos residen en servidores que tienen el cifrado de nivel de volumen habilitado. 

Puesto que {{site.data.keyword.composeForScyllaDB}} es un servicio gestionado, los operadores de {{site.data.keyword.cloud_notm}} Compose pueden ver datos. Además del cifrado de disco, debe cifrar información personal antes de almacenarla en la base de datos o utilizar extensiones o características para habilitar el cifrado en la propia base de datos. Si bien estos métodos pueden afectar a la usabilidad o al rendimiento, es una buena práctica asegurarse de que la información personal esté protegida con cifrado.

## Escalado automático

Los recursos se escalan automáticamente basándose en el uso de almacenamiento de disco total de las bases de datos Scylla. La memoria se basa en una proporción de disco suministrado de 1:10. Estos recursos se empaquetan como unidades.

El escalado automático está diseñado para responder a las tendencias a corto y medio plazo de la base de datos. El servicio se ha comprobado cada hora. Si se está quedando corto en el espacio de disco, se asignarán más unidades al despliegue. 

El escalado automático no reduce los despliegues en los que el uso de disco/memoria se ha reducido. Los recursos suministrados a las bases de datos permanecen para sus necesidades futuras, o hasta que reduzca el despliegue manualmente.
{: .tip}


