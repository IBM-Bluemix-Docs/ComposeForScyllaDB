---

copyright:
  years: 2018
lastupdated: "2018-05-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de cqlsh
{: #using-cqlsh}

Puede utilizar `cqlsh` para conectarse directamente a {{site.data.keyword.composeForScyllaDB_full}}.

En el momento de publicar esta guía, `cqlsh` es un programa de Python 2.7 y no se ejecuta con Python 3.
{: .tip}

Como `cqlsh` es una aplicación Python, puede ejecutar `pip install cqlsh` para instala la última versión del mandato.

También puede obtener `cqlsh` de una distribución completa de Cassandra descargándola de [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/), que ofrece contenido binario, fuente e instrucciones sobre cómo instalar Cassandra en distribuciones Debian (apt) y Red Hat (RPM) de Linux. Descargue un release binario y desempaquete el archivo en un directorio. Conéctese mediante `cd` a dicho directorio y encontrará `cqlsh` en el directorio `bin`.

Cómo se obtiene esta herramienta en el dispositivo local depende del sistema operativo local. Lo más sencillo es instalar el último release de Cassandra utilizando un método adecuado para su sistema operativo (la última versión todavía da soporte a la versión 2.1.8, que es la de Scylla) y entonces utilizar el mandato incorporado `cqlsh`. 

En un Mac, por ejemplo, puede utilizar `homebrew` para instalar Cassandra escribiendo `brew install cassandra`.

## Archivos cqlshrc

Al ejecutar el mandato `cqlsh`, se carga `$HOME/.cassandra/cqlshrc` de forma predeterminada, que puede definir muchos parámetros que no se pueden definir en la línea de mandatos. Puede editar el archivo predeterminado para establecer parámetros, y cualquier cambio que realice se aplicará a todas las sesiones, a menos que se sobrescriban de la línea de mandatos.

Si ejecuta varias instancias de Scylla en diferentes entornos, puede crear un archivo `cqlshrc` independiente y utilizarlo añadiendo `--cqlshrc=[filename]` al mandato.


Para obtener un ejemplo de un archivo cqlshrc completo, consulte el [ejemplo de Apache de cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). 

### cqlshrc para {{site.data.keyword.composeForScyllaDB}}

Un archivo `cqlshrc` mínimo para conectarse al servicio tiene este aspecto:

```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
version=SSLv23
certfile = ~/path_to_your/lechain.pem
validate = true
```

Escriba la información que se proporciona en las _Series de conexión_ del servicio y guarde el archivo como `example.cqlshrc`.

Ahora puede iniciar sesión en la base de datos de Scylla ejecutando `cqlsh --ssl --cqlshrc=example.cqlshrc` desde la línea de mandatos. Tenga en cuenta que, aunque `cqlshrc` tiene una opción para especificar `ssl=true`, parece que se omite, por lo que es necesario utilizar el distintivo `--ssl`.

## TLSv1.2

`cqlsh` no utiliza TLSv1.2 como valor predeterminado al negociar una conexión cifrada. Para conectarse correctamente a un despliegue de Compose Scylla, la versión de TLS debe estar especificada en el entorno que ejecuta `cqlsh`. Para obtener la mayor compatibilidad (y un poco de prueba de futuro para TLSv1.3), establezca `SSL_VERSION` en el entorno a SSLv23 utilizando `export SSL_VERSION=SSLv23` o colocando `SSL_VERSION=SSLv23` antes del mandato de conexión `cqlsh`.

También puede establecer la versión específicamente en TLSv1.2 utilizando `export SSL_VERSION=TLSv1_2` o colocando `SSL_VERSION=TLSv1_2` antes del mandato de conexión `cqlsh`.

## Conexión sin validación

La validación está habilitada de forma predeterminada, pero se puede conectar a Scylla con TLS/SSL sin validar el host remoto. Sin embargo, esto no se recomienda para los sistemas de producción. La validación se controla con la variable de entorno `SSL_VALIDATE` o con los valores de SSL en el archivo cqlshrc. Establezca `SSL_VALIDATE` en false (`export SSL_VALIDATE=false`) en el entorno para inhabilitar la validación.

Como alternativa, puede editar el archivo cqlshrc para inhabilitar la validación.

```
[ssl]  
validate = false
```

Si desea inhabilitar la validación para un único mandato `cqlsh`, incluya `SSL_VALIDATE=false` antes del mandato de conexión de `cqlsh`. 

Todavía tiene que indicar a `cqlsh` que utilice TLSv1.2, incluso cuando la validación no esté habilitada. Si no ha establecido la versión de SSL en el entorno y no desea validar, la conexión debe parecerse a este ejemplo:

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtención y utilización del certificado

Si desea utilizar cqlsh y verificar el host remoto, debe obtener el certificado, guardarlo en algún lugar accesible y luego indicar a cqlsh la vía de acceso al certificado.

Establezca la variable de entorno `SSL_CERTFILE` en la vía de acceso y el nombre de archivo del certificado guardado para habilitarla para todas las invocaciones posteriores de `cqlsh`. 

De forma alternativa, puede añadir lo siguiente al archivo cqlshrc.

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Si sólo desea utilizar un certificado concreto para un único mandato `cqlsh`, escriba `SSL_CERTFILE = ~/path_to_your/lechain.pem` al principio del mandato de conexión de cqlsh. Por ejemplo:

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Iniciación a cqlsh

Si escribe `HELP`, verá que el shell tiene mucha capacidad. Todos los mandatos tienen también la finalización de `TAB`. Por ejemplo:

1. Escriba `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Debería ver las opciones para la clase de réplica.
2. Elija `SimpleStrategy` porque el clúster no abarcará varios centros de datos.
3. Pulse `<TAB><TAB>` de nuevo y escriba 3 para el valor de `replication_factor`. Cierre las llaves con `}` y finalice la sentencia con `;<enter>`.

Ha creado el primer espacio de clave, que de forma predeterminada realiza una réplica de los datos en los tres nodos del clúster.

```sql
scylla@cqlsh> CREATE KEYSPACE example_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3 };
scylla@cqlsh> use example_keyspace ;
scylla@cqlsh:example_keyspace> create table names (
                    ... name_id uuid,
                    ... last_name text,
                    ... first_name text,
                    ... PRIMARY KEY(name_id)
                    ... );
scylla@cqlsh:example_keyspace> 
```

Ahora puede utilizar el espacio de clave:

```sql 
USE my_new_keyspace;
```

Ahora el shell muestra que el indicador de mandatos utiliza el espacio de clave de forma predeterminada. Cada tabla debe tener un espacio de clave.

Escriba el siguiente mandato `CREATE TABLE` en el shell para crear una tabla que llenar con ejemplos.

```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
