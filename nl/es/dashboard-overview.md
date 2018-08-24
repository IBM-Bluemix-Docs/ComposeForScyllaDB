---

Copyright:
  years: 2017,2018
lastupdated: "2018-05-07"
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

El tipo de base de datos que ofrece el servicio y la versión de la base de datos que utiliza el servicio. Si hay disponible una versión de base de datos más reciente, se mostrará una notificación, junto con un enlace a la sección [Actualizar versión](/docs/services/ComposeForScyllaDB/dashboard-settings.html#upgrade-version) del panel de control de servicio.

### ID

Un identificador interno para el servicio.

### Uso

El tamaño de la base de datos y la cantidad de almacenamiento que proporciona su plan de servicio.

## Trabajos actuales

Hacer cambios administrativos a su servicio (como escalado, o tomar una copia manual) inicia un trabajo. Mientras se está ejecutando un trabajo, el panel _Trabajos actuales_ aparecerá en la página _Visión general_, mostrando el nombre del trabajo y una barra de progreso. Cuando el trabajo se ha completado, el panel _Trabajos actuales_ ya no aparecerá en la página _Visión general_.

## Series de conexión

Encontrará cada serie de conexión para el servicio en un separador diferente en el panel _Series de conexión_.

### HTTPS

Algunas bibliotecas de cliente pueden utilizar una serie de conexión con formato URI, que contiene toda la información necesaria para que se conecten otras bibliotecas. Encontrará información sobre cómo utilizar la serie de conexión para conectarse en el apartado [Conexión de una aplicación externa](./connecting-external.html).

### Línea de mandatos

La **línea de mandatos** es un mandato preformateado que invoca `cqlsh` con los parámetros correctos. El mandato visualizado incluye los distintivos necesarios (`--ssl` y `--cqlversion`).  Para poderla utilizar, debe instalar las herramientas de cliente de PostgreSQL en el sistema local. Encontrará información sobre cómo hacerlo en el apartado [Conexión de una aplicación externa](./connecting-external.html).

### Correlaciones
Estas correlaciones de conversión de direcciones se pueden utilizar en aplicaciones que requieren alta disponibilidad y pueden utilizar el descubrimiento automático para buscar nodos en el clúster. Convierten las direcciones del portal externo en direcciones del clúster interno para los controladores de cliente que utilizan esta característica.

### Socks y Nodetool
Para habilitar la administración de nodetool de Scylla, el servicio viene con una cápsula SSH configurada como un proxy SOCKS. Debe añadir una clave SSH al despliegue para utilizar el proxy. Puede encontrar más información en [Utilización de Nodetool](./scylla-nodetool.html).


## API de administración de instancias

Puede gestionar el servicio de {{site.data.keyword.composeForScyllaDB}} a través de la API de {{site.data.keyword.cloud_notm}} Compose.

### Punto final de la fundación

El punto final de la fundación se compone de la región del servicio en el que reside y el ID de instancia de servicio. Estará al principio de cada punto final.

### ID de despliegue

El ID de despliegue es necesario para la mayoría de las llamadas, e identifica la instancia de despliegue específica.

### Referencia

Para obtener más documentación y referencia para utilizar la API de {{site.data.keyword.cloud_notm}}, en todos los servicios de {{site.data.keyword.cloud_notm}} Compose, lea [La API de {{site.data.keyword.cloud_notm}} Compose](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
