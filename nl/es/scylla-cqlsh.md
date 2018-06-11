---

copyright:
  years: 2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilización de cqlsh
{: #using-cqlsh}

Puede conectarse directamente a {{site.data.keyword.composeForScyllaDB_full}} utilizando `cqlsh`.
Hay varias formas de realizar la instalación. Como es una aplicación Python, al ejecutar `pip install cqlsh` se instalará la última versión del mandato. En el momento de publicar esta guía, `cqlsh` es un programa de Python 2.7 y no se ejecuta con Python 3.

También es posible obtener `cqlsh` de una distribución completa de Cassandra. Se puede obtener de [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/), que ofrece contenido binario, fuente e instrucciones sobre cómo instalar Cassandra en distribuciones Debian (apt) y Red Hat (RPM) de Linux. Descargue un release binario y desempaquete el archivo en un directorio. Conéctese mediante `cd` a dicho directorio y encontrará `cqlsh` en el directorio `bin`.

Cómo se obtiene esta herramienta en el dispositivo local depende de la plataforma local. Lo más sencilla es instalar el último release de Cassandra utilizando un método adecuado para su plataforma (la última versión todavía da soporte a la versión 2.1.8, que es la de Scylla) y utilizar el mandato incorporado `cqlsh`. 

En un Mac, por ejemplo, instale Cassandra con `homebrew` escribiendo `brew install cassandra`.

## Acerca de los archivos cqlshrc
Al ejecutar el mandato `cqlsh`, carga un archivo, `$HOME/.cassandra/cqlshrc` de forma predeterminada, que puede definir muchos parámetros que no se pueden definir en la línea de mandatos. [Ejemplo de Apache de cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). Puede editar el archivo predeterminado para establecer parámetros. Se aplicarán a todas las sesiones a no ser que se sustituyan en la línea de mandatos.

Si ejecuta varias instancias de Scylla en diferentes entornos, puede crear un archivo `cqlshrc` independiente y hacer que `cqlsh` lo utilice añadiendo `--cqlshrc=[filename]` al mandato.

### cqlshrc para {{site.data.keyword.composeForScyllaDB}}
Este es un archivo `cqlshrc` de ejemplo mínimo para conectarse al servicio:
```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Escriba la información indicada en las _Series de conexión_ del servicio y guarde este archivo como example.cqlshrc.
Ejecute `cqlsh --ssl --cqlshrc=example.cqlshrc` en la línea de mandatos e iniciará sesión en la base de datos de Scylla. Tenga en cuenta que, aunque `cqlshrc` tiene una opción para especificar `ssl=true`, parece ser que se ignora, por lo que es necesario utilizar el distintivo `--ssl`.

La versión de ssl no puede establecerse en el archivo `cqlshrc`, por lo que tendrá que definir que el entorno utilice TLSv1.2 o añada SSL_VERSION=TLSv1_2 a la parte frontal del mandato de conexión de `cqlsh`.

## TLSv1.2

`cqlsh` no utiliza TLSv1.2 como valor predeterminado al negociar una conexión cifrada. Para conectarse correctamente a un despliegue de Compose Scylla, la versión de TLS debe estar especificada en el entorno que ejecuta `cqlsh`. Defina SSL_VERSION en el entorno mediante:
`export SSL_VERSION=TLSv1_2`
o escriba `SSL_VERSION=TLSv1_2` antes del mandato de conexión de `cqlsh`.

## Conexión sin validación

La forma más sencilla de conectarse a Scylla con TLS/SSL es sin validar el host remoto. No se recomienda para entornos de producción. De forma predeterminada, la validación está habilitada. La validación se controla con la variable de entorno `SSL_VALIDATE` o con los valores de SSL en el archivo cqlshrc. Establezca `SSL_VALIDATE` en false en el entorno para inhabilitar la validación:
`export SSL_VALIDATE=false`

De forma alternativa, puede añadir lo siguiente al archivo cqlshrc.

```
[ssl]  
validate = false
```

Si sólo desea inhabilitarla en un único mandato `cqlsh`, escriba `SSL_VALIDATE=false` antes del mandato de conexión de `cqlsh`. 

Aunque no se valide el certificado, le tiene que indicar igualmente a `cqlsh` que utilice TLSv1.2. Si no ha establecido la versión de SSL en el entorno y no desea validar, la conexión debe parecerse a este ejemplo:

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtención y utilización del certificado

Si desea utilizar cqlsh y verificar el host remoto, deberá obtener el certificado, guardarlo en algún lugar accesible y luego indicar a cqlsh la vía de acceso al certificado. En la página [Scylla y certificados](doc:scylla-and-certificates) se guarda una copia del certificado, así como detalles sobre su contenido y su código fuente. Cree un archivo de certificado o guarde una copia local. 

Establezca la variable de entorno `SSL_CERTFILE` en la vía de acceso y el nombre de archivo del certificado guardado para habilitarla para todas las invocaciones posteriores de `cqlsh`. 

De forma alternativa, puede añadir lo siguiente al archivo cqlshrc.
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Si sólo desea utilizar un certificado concreto para un único mandato `cqlsh`, escriba `SSL_CERTFILE = ~/path_to_your/lechain.pem` al principio del mandato de conexión de cqlsh. Por ejemplo:

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Utilización de cqlsh

Si escribe `HELP`, verá que el shell tiene mucha capacidad. Y lo mejor es que todos estos mandatos también tienen la función `TAB`. Por ejemplo:
1. Escriba `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Debería ver las opciones para la clase de réplica.
2. Aquí puede elegir `SimpleStrategy` porque el clúster no abarcará varios centros de datos.
3. Pulse `<TAB><TAB>` de nuevo y escriba 3 para replication_factor. Cierre las llaves con `}` y finalice la sentencia con `;<enter>`.

Acaba de crear el primer espacio de clave, que de forma predeterminada realiza una réplica de los datos en los tres nodos del clúster.
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
