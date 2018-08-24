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

# Configuración de conexión
{: #connection-configuration}

Las conexiones de base de datos {{site.data.keyword.composeForScyllaDB_full}} están gestionadas por 3 portales HAProxy. Cada portal tiene 64 MB de memoria.

La migración tras error en el lado del cliente es responsabilidad del diseñador de aplicaciones. La mayoría de los controladores de Scylla son conscientes del clúster, ya que puede proporcionar una serie de conexión, las tres cadenas de conexión, las correlaciones de conexión o alguna combinación de estas para manejar varias series de conexión y una migración tras error ordenada automáticamente. Si no utiliza un controlador que gestione por completo los errores de migración tras error, debe:

* Utilizar un controlador que acepte varios puntos finales en su configuración de conexión.
* Asegurarse de que las llamadas de aplicaciones para leer o escribir en la base de datos reaccionen a los errores adecuadamente. Entre las opciones, se incluyen:
  + Registrar el error y reconectar.
  + Reconectar y reintentar la operación que ha provocado el error.
  + Poner en cola la operación que ha fallado y planificar la reconexión.

## Cifrado en tránsito

Todos los portales HAProxy de {{site.data.keyword.composeForScyllaDB}} tienen habilitado TLS/SSL y dan soporte a TLS versión 1.2. Los certificados para el servicio son certificados de Let's Encrypt. Determinados controladores y la interfaz de línea de mandatos, cqlsh, necesitan una copia de la cadena de certificados para conectarse con el servicio. Para obtener más información, consulte [Uso de certificados](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)

## Límites de conexión

Las conexiones de base de datos tienen un límite de conexión de 1000 conexiones por portal. El exceso del límite de conexión hará que el servicio no responda. Puede gestionar las conexiones desde la aplicación a través de la característica de agrupación de conexiones del controlador.

## Tiempos de espera del proxy

Los portales HAProxy gestionan el ciclo de vida de las conexiones entre el servidor y el cliente. Los detallamos aquí como una referencia para los escritores de controladores y otros que tienen interés en la actividad de bajo nivel de las conexiones de base de datos de {{site.data.keyword.composeForScyllaDB}}.

Valor | Valor | Se aplica
----------|-----------|-----------
cliente | 5m | Cuando se espera que el cliente reconozca o envíe datos.
connect | 4s | Cuando se realiza una conexión entre el proxy y el servidor.
servidor | 5m | Cuando se espera que el servidor reconozca o envíe datos.
tunnel | 1 día | Cuando se establece una conexión bidireccional entre un cliente y un servidor, y la conexión permanece inactiva en ambas direcciones.
client-fin | 1h | Cuando se espera que el cliente reconozca o envíe datos mientras una dirección ya está cerrada.
check | 5s | Como una comprobación de tiempo de espera secundaria, cuando se realiza una conexión entre el proxy y el servidor.

{: caption="Tabla 1. Tiempos de espera de HAProxy de Scylla" caption-side="top"}
