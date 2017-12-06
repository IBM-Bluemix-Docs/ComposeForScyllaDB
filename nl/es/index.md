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

# Iniciación a Compose for ScyllaDB
{: #getting-started-with-compose-for-scylladb}

ScyllaDB es un sustituto de la base de datos distribuida de columna ancha Cassandra. ScyllaDB está escrito en C++, en lugar de Java de Cassandra, para conseguir una mejor utilización de recursos, que puede llegar a un rendimiento diez veces superior en las pruebas de referencia. Además de mantener la compatibilidad con la herramienta Cassandra y los archivos de datos, ScyllaDB añade capacidades de ajuste automático. {{site.data.keyword.composeForScyllaDB_full}} amplía las capacidades de ScyllaDB gestionándolo para los usuarios, ofreciendo un sistema de despliegue sencillo y de escalado automático que proporcione alta disponibilidad y redundancia, además de copias de seguridad automatizadas.
{:shortdesc}

**Nota:** {{site.data.keyword.composeForScyllaDB_full}} no da acceso a la IU de Compose. Consulte [Soporte de Compose en {{site.data.keyword.cloud}}](https://help.compose.com/docs/bluemix-compose-support) para obtener más detalles.

## Creación de una instancia del servicio Compose for ScyllaDB

[Cree una instancia de {{site.data.keyword.composeForScyllaDB}}](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/).

Al crear una instancia del servicio, seleccione un nombre para el servicio y un nombre de credencial. Deje el servicio desenlazado; puede conectar una aplicación al servicio más adelante utilizando las credenciales proporcionadas cuando se proporcione el servicio. Los distintos valores de credenciales se listan en la sección "Credenciales disponibles".

Cuando suministre la instancia de {{site.data.keyword.composeForScyllaDB}}, puede elegir los planes *Estándar* o *Empresa*. Con el plan *Empresa*, puede suministrar la instancia de {{site.data.keyword.composeForScyllaDB}} en un clúster disponible de {{site.data.keyword.composeEnterprise}}. {{site.data.keyword.composeEnterprise}} proporciona la seguridad y nivel de aislamiento necesarios para el cumplimiento de las reglas empresariales y utiliza redes dedicadas para garantizar el rendimiento de las bases de datos desplegadas. Consulte la [documentación de Compose Enterprise](../ComposeEnterprise/index.html) para ver más detalles.

## Gestión de Compose for ScyllaDB

Puede gestionar el servicio desde el panel de control del servicio. Aquí encontrará información sobre su base de datos de {{site.data.keyword.cloud_notm}} Compose y cómo conectarse a la misma. También puede:

- gestionar copias de seguridad
- asignar más recursos para el servicio 
- utilizar listas blancas para restringir el acceso a las bases de datos. 

Para obtener más información, consulte [Valores](./dashboard-settings.html).

## Conexión a Compose for ScyllaDB

Puede conectarse a su servicio utilizando las credenciales que se crean junto con el servicio, o con las series de conexión y la línea de mandatos que se proporcionan en el separador *Visión general* del panel de control del servicio.

## Conexión de una aplicación {{site.data.keyword.cloud_notm}} a Compose for ScyllaDB

Para conectar una aplicación {{site.data.keyword.cloud_notm}} al servicio, utilice las credenciales que se crean junto con el servicio. Encontrará información sobre cómo conectar una aplicación {{site.data.keyword.cloud_notm}} a un servicio {{site.data.keyword.composeForScyllaDB}} en el apartado [Conexión de una aplicación {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Conexión a Compose for ScyllaDB desde fuera de {{site.data.keyword.cloud_notm}}

Si desea conectarse a {{site.data.keyword.composeForScyllaDB}} desde fuera de {{site.data.keyword.cloud_notm}}, puede utilizar las series de conexión proporcionadas o la línea de mandatos. Encontrará información sobre cómo conectar en el apartado [Conexión de una aplicación externa](./connecting-external.html).
