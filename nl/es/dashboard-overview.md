---

Copyright:
  Years: 2017
lastupdated: "2017-09-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Visión general del servicio

La página _Visión general_ muestra información sobre la base de datos de {{site.data.keyword.cloud}} Compose. La visión general incluye información de identificación esencial y el uso actual de recursos. También encontrará una sección correspondiente a las series de conexión que puede utilizar con las herramientas o las herramientas que puede usar para conectarse a la base de datos.

## Detalles de despliegue

El panel _Detalles de despliegue_ muestra detalles del servicio.

![Detalles de despliegue](./images/scylla-deployment-details.png "Una vista del panel Detalles de despliegue")

### Tipo

El tipo de base de datos que ofrece el servicio y la versión de la base de datos que utiliza el servicio.

### Nombre

Un identificador interno para el servicio.

### Uso

El tamaño de la base de datos y la cantidad de almacenamiento que proporciona su plan de servicio.


## Series de conexión

Encontrará cada serie de conexión para el servicio en un separador diferente en el panel _Series de conexión_.

### HTTPS

Algunas bibliotecas de cliente pueden utilizar una serie de conexión con formato URI, que contiene toda la información necesaria para que se conecten otras bibliotecas. Encontrará información sobre cómo utilizar la serie de conexión para conectarse en el apartado [Conexión de una aplicación externa](./connecting-external.html).

### Línea de mandatos

La **línea de mandatos** es un mandato preformateado que invocará `cqlsh` con los parámetros correctos. El mandato visualizado incluye los distintivos necesarios (`--ssl` y `--cqlversion`).  Para poderla utilizar, deberá tener las herramientas de cliente de PostgreSQL instaladas en el sistema local. Encontrará información sobre cómo hacerlo en el apartado [Conexión de una aplicación externa](./connecting-external.html).

### Correlaciones
Estas correlaciones de conversión de direcciones se pueden utilizar en aplicaciones que requieren alta disponibilidad y pueden utilizar el descubrimiento automático para buscar nodos en el clúster. Convierten las direcciones del portal externo en direcciones del clúster interno para los controladores de cliente que utilizan esta característica.

### Certificado SSL

El servicio Compose {{site.data.keyword.cloud_notm}} le ofrece un certificado SSL que puede utilizar para conectar con la base de datos.
