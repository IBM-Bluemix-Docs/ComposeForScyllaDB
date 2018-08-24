---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Conexión de una aplicación {{site.data.keyword.cloud_notm}}

Para conectar una aplicación {{site.data.keyword.cloud}} al servicio, utilice las credenciales de servicio que se crean cuando se suministra el servicio.

## Conexión con la app de ejemplo 'Hello World'

La app de ejemplo [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) muestra cómo utilizar Node.js para conectar a un servicio de {{site.data.keyword.composeForScyllaDB}} utilizando las credenciales proporcionada. La aplicación crea, lee y escribe en una base de datos.

Descargue la app de ejemplo y siga las instrucciones del archivo readme. A continuación, en la página de detalles de la aplicación de {{site.data.keyword.cloud_notm}}, pulse **Ver APP** para ver el contenido de la tabla *ejemplos*.

## Credenciales disponibles

Nombre de campo|Descripción
----------|-----------
`db_type`|El tipo de base de datos que ofrece el servicio, en este caso `scylla`.
`uri_cli_1`|Una línea de mandatos de shell `cqlsh` secundaria que se conecta a la instancia de la base de datos.
`maps`|Un mapa de conexión de ScyllaDB que proporciona la información que necesitan varios controladores para asociar direcciones IP internas con los nombres DNS externos de una base de datos de ScyllaDB.
`name`|El nombre del despliegue de la base de datos.
`uri_cli`|Una línea de mandatos de shell `cqlsh` que se conecta a la instancia de la base de datos.
`uri_direct_2`|Un tercer URI que se puede utilizar para conectarse al servicio. El formato es el mismo que `uri`.
`uri_direct_1`|Un URI secundario que se puede utilizar para conectarse al servicio. El formato es el mismo que `uri`.
`ca_certificate_base64` `(opcional)`|Un certificado codificado en base64 y firmado automáticamente que se utiliza para confirmar que una aplicación se está conectando al servidor apropiado. El certificado solo está presente en los servicios que tienen una firma autofirmada en lugar de un certificado Let's Encrypt. Debe decodificar la clave antes de poder utilizarla, como se muestra en la aplicación de ejemplo.
`deployment_id`|Un identificador interno para el servicio, tal como se ha creado en Compose.
`uri_cli_2`|Una tercera línea de mandatos de shell `cqlsh` que se conecta a la instancia de la base de datos.
`uri`|El URI que se utiliza al conectarse al servicio, que incluye el esquema (`scylla:`), la contraseña, el nombre de host del servidor, el número de puerto al que se conecta y el nombre de la base de datos.
{: caption="Tabla 1. Credenciales de Compose for ScyllaDB" caption-side="top"}

**Nota:** Dos portales `haproxy` proporcionan acceso al servicio Scylla. `uri`, `uri_direct_1` y `uri_direct_2` pueden utilizarse para conectarse. En las aplicaciones, conmute entre `uri`, `uri_direct_1` y `uri_direct_2` para gestionar respuestas a fallos de conexión.
