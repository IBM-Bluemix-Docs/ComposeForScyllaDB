---

copyright:
  years: 2017,2018
lastupdated: "2018-02-28"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando um aplicativo externo
{: #connecting-external-app}

É possível localizar as informações que você precisa para se conectar ao {{site.data.keyword.composeForScyllaDB_full}} na página *Visão geral* de seu serviço {{site.data.keyword.composeForPostgreSQL_full}}.

## Conectando-se por meio da JVM

Um dos drivers mais avançados para o Cassandra é o driver Java. Isso faz sentido considerando que o Cassandra é escrito em Java. O que se segue é um script [Groovy](http://www.groovy-lang.org/documentation.html#gettingstarted). Para aqueles que utilizam praticamente qualquer linguagem da JVM, converter de Groovy para a linguagem de sua escolha deve ser relativamente simples:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
@Grab('org.slf4j:slf4j-log4j12')

import com.datastax.driver.core.BoundStatement
import com.datastax.driver.core.Cluster
import com.datastax.driver.core.Host
import com.datastax.driver.core.PreparedStatement
import com.datastax.driver.core.Row
import com.datastax.driver.core.Session

import static java.util.UUID.randomUUID

Cluster cluster = Cluster.builder()
    .addContactPointsWithPorts(
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15399 ),
        new InetSocketAddress("aws-us-east-1-portal9.dblayer.com", 15401 ),
        new InetSocketAddress("aws-us-east-1-portal6.dblayer.com", 15400 )
    )
    .withCredentials("scylla", "XOEDTTBPZGYAZIQD")
    .build()

Session session = cluster.connect("my_new_keyspace")

PreparedStatement myPreparedInsert = session.prepare(
  """INSERT INTO my_new_table(my_table_id, last_name, first_name)
     VALUES (?,?,?)""")

BoundStatement myInsert = myPreparedInsert
    .bind(randomUUID(), "Hutton", "Hays")

session.execute(myInsert)

session.close()
cluster.close()
```

Para começar, vamos puxar o driver mais recente do Cassandra:

```java
@Grab('com.datastax.cassandra:cassandra-driver-core:3.1.0')
```

Após todas as importações, usamos um `Cluster.builder()` para construir a configuração. Apenas um dos `ContactPoint`s é usado para conectar. Por meio dessa conexão, outros nós no cluster são descobertos. Se este `ContactPoint` for inatingível em `connect`, outro será usado, que é por isso que incluímos todos os três.

`PreparedStatement`s podem ser familiares porque são análogos a recursos de outros DBs com o mesmo nome. A instrução é analisada e retida no servidor pronto para ser usado repetidamente. As chamadas a seguir para `bind` e `execute` preenchem e enviam os dados para o servidor para execução real. Embora existam métodos mais simples para execução única, é bom destacar esse recurso tão útil.

Para provar que o script funciona, volte para o seu `cqlsh` e consulte a tabela:
![Results from `SELECT` in `cqlsh`.](./images/results_select_java.png "Results from Select")

## Conectando-se do Python

Para conectar o seu aplicativo python, use o [DataStax Python Driver](https://github.com/datastax/python-driver). A instalação pode ser feita por meio do pip:

```shell
pip install cassandra-driver
```

Isso puxa o driver com um gerenciador de pacote python `pip`. O driver espera um certificado para uso quando o TLS/SSL está ativado e instruções para usar um certificado com o seu serviço estão na página [Usando certificados do LE](./scylla-certificates.html). Todas as outras informações das quais o driver precisa estão nas _Sequências de conexão_ do serviço _Visão geral_.

O seguinte executa de forma muito semelhante ao código Java a preparação de uma instrução e a execução de uma inserção:

```python
import ssl
import uuid
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider

ssl_options = {
    "ca_certs":"/path_to_cert/lecert.pem",
    "ssl_version":ssl.PROTOCOL_TLSv1_2
}

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='your_password')

cluster = Cluster(
            contact_points = [
                "portal-59b813def16d6e0014003b45224-8.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",  
                "portal-59b813def16d6e0014003b47186-6.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com",  
                "portal-59b813def16d6e0014003b49208-7.bmix-dal-ys1-fd6a5b7e-e120-43f3-95ea-e40028e540a8.composeci-us-ibm-com.composedb.com"
                ],
            port = 15401,
            auth_provider = auth_provider,
            ssl_options=ssl_options)

session = cluster.connect('my_new_keyspace')

my_prepared_insert = session.prepare("""
    INSERT INTO my_new_table(my_table_id, first_name, last_name)
    VALUES (?, ?, ?)""")

session.execute(my_prepared_insert, [uuid.uuid4(), 'Snake', 'Hutton'])
```

Para verificar novamente, vamos executar o mesmo comando `SELECT`:

![Results from `SELECT` in `cqlsh`.](./images/results_select_python.png "Results from Select")

## Conectando-se do Node.js

Use o node package manager (npm) para instalar o driver e a biblioteca `uuid` necessária.

```shell
npm install cassandra-driver
npm install uuid
```

 O código é semelhante aos exemplos anteriores:

```javascript
var cassandra = require('cassandra-driver')
var authProvider = new cassandra.auth.PlainTextAuthProvider('scylla', 'XOEDTTBPZGYAZIQD')
var uuid = require('uuid')

client = new cassandra.Client({
                        contactPoints: [
                          "aws-us-east-1-portal9.dblayer.com:15399",
                          "aws-us-east-1-portal9.dblayer.com:15401",
                          "aws-us-east-1-portal6.dblayer.com:15400"
                        ],
                        keyspace: 'my_new_keyspace',
                        authProvider: authProvider});

client.execute("INSERT INTO my_new_table(my_table_id, first_name, last_name) VALUES(?,?,?)",
               [uuid.v4(), "V8", "Hutton"],
               { prepare: true },
               function(err, result) {
                 if(err) { console.error(err); }
                 console.log("success")
               });

```

Mais uma vez, o código conecta, prepara e executa uma instrução de inserção. Verifique o código com o comando SELECT:

![Results from `SELECT` in `cqlsh`.](./images/results_select_node.png "Results from Select")
