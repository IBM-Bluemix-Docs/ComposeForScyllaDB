---

copyright:
  years: 2016,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conexión de una aplicación {{site.data.keyword.cloud_notm}}

Para conectar una aplicación {{site.data.keyword.cloud}} al servicio, utilice las credenciales de servicio que se crean cuando se suministra el servicio. La app de ejemplo muestra cómo utilizar Node.js para conectar a un servicio de {{site.data.keyword.composeForScyllaDB_full}} utilizando las credenciales y cómo crear una base de datos y leer y escribir en la base de datos.

## Conexión mediante la app de ejemplo 'Hello World'

La app de ejemplo [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) muestra cómo utilizar Node.js para conectar a un servicio de {{site.data.keyword.composeForScyllaDB}} utilizando las credenciales proporcionada. La aplicación crea, lee y escribe en una base de datos.

Descargue la app de ejemplo y siga las instrucciones del archivo readme. A continuación, en la página de detalles de la aplicación de {{site.data.keyword.cloud_notm}}, pulse **Ver APP** para ver el contenido de la tabla *ejemplos*.

## Credenciales disponibles

Nombre de campo|Descripción
----------|-----------
`db_type`|El tipo de base de datos que ofrece el servicio, en este caso `scylla`.
`uri_cli_1`|Una línea de mandatos de shell `cqlsh` alternativa que se conecta a la instancia de la base de datos.
`maps`|Un mapa de conexión de ScyllaDB que proporciona la información que necesitan varios controladores para asociar direcciones IP internas con los nombres DNS externos de una base de datos de ScyllaDB.
`name`|El nombre del despliegue de la base de datos.
`uri_cli`|Una línea de mandatos de shell `cqlsh` que se conecta a la instancia de la base de datos.
`uri_direct_2`|Un URI alternativo que se puede utilizar para conectarse al servicio. Formateado en cuanto a `uri`.
`uri_direct_1`|Un URI alternativo que se puede utilizar para conectarse al servicio. Formateado en cuanto a `uri`.
`ca_certificate_base64`|Un certificado firmado automáticamente que se utiliza para confirmar que una aplicación se está conectando al servidor apropiado. El certificado está codificado como base64.
`deployment_id`|Un identificador interno para el servicio, tal como se ha creado en Compose.
`uri_cli_2`|Una línea de mandatos de shell `cqlsh` alternativa que se conecta a la instancia de la base de datos.
`uri`|El URI que se utiliza al conectarse al servicio, que incluye el esquema (`scylla:`), la contraseña, el nombre de host del servidor, el número de puerto al que se conecta y el nombre de la base de datos.
{: caption="Tabla 1. Credenciales de Compose for ScyllaDB" caption-side="top"}
