---

copyright:
  years: 2017,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando um aplicativo externo
{: #connecting-external-app}

É possível localizar as informações que você precisa para se conectar ao {{site.data.keyword.composeForScyllaDB_full}} na página *Visão geral* de seu serviço {{site.data.keyword.composeForPostgreSQL_full}}.

É possível se conectar diretamente ao {{site.data.keyword.composeForScyllaDB}} usando `cqlsh`. Como você obtém essa ferramenta em seu dispositivo local depende de sua plataforma local. O mais fácil é instalar a liberação mais recente do Cassandra usando um método apropriado para sua plataforma (as versões mais recentes ainda suportam a versão 2.1.8, que é o que Scylla é) e usar o comando `cqlsh` integrado.

Em um Mac, por exemplo:

1. Instale o Cassandra com `homebrew` digitando `brew install cassandra`.
2. Copie qualquer um dos comandos da página Visão geral:

  ![Example `cqlsh` connection string.](./cqlsh_connection_string "Example cqlsh connection string")

3. Cole o comando em seu shell para executá-lo:

  ![The `cqlsh` shell.](./cqlsh_shell.png "The cqlsh shell")

4. Se você digitar `HELP`, será possível ver que o shell tem muita capacidade. O melhor é que todos esses comandos têm a conclusão de `TAB` também.
5. Digite `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Você deverá ver as opções para a classe de replicação.
6. É possível escolher `SimpleStrategy` aqui porque o cluster não abrangerá múltiplos data centers.
7. Pressione `<TAB><TAB>` novamente e insira 3 para o replication_factor. Em seguida, feche a chave com `}` e conclua a instrução com `;<enter>`.

  Você criou seu primeiro KEYSPACE e o padronizou para replicar seus dados para todos os três nós em seu cluster.

8. Agora é possível usar o keyspace:

  ```sql
  USE my_new_keyspace;
  ```

  O shell agora mostra que seu prompt de comandos está usando seu keyspace por padrão:

  ![Running `CREATE KEYSPACE` and `USE`.](./images/running_create_keyspace_use.png "Running `CREATE KEYSPACE` and `USE`")

  Cada tabela tem que ter um keyspace. Quando você criar um no shell aqui, ele padronizará para `my_new_keyspace`.

  Embora o Scylla/Cassandra tenha se desenvolvido para ter uma linguagem de esquema muito semelhante ao SQL, não é realmente o caso. Diferente de um RDBMS, uma linha aqui é muito mais como uma consulta de valor da chave. Acontece que o valor tem um esquema flexível que estamos prestes a definir:

9. Digite o comando `CREATE TABLE` no shell para criar um local para preencher com exemplos.

  ```sql
  CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
  );
  ```

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

O suporte para linguagens diferentes do Java é muito sólido também. Python é um grande exemplo. `cqlsh` é escrito até mesmo em Python. Então, não se engane, o suporte aqui está mais do que atualizado:

```shell
pip install cassandra-driver
```

Isso puxa o driver com um gerenciador de pacote python `pip`. O seguinte executa de forma muito semelhante ao código Java a preparação de uma instrução e a execução de uma inserção:

```python
from cassandra.cluster import Cluster
from cassandra.auth import PlainTextAuthProvider
import uuid

auth_provider = PlainTextAuthProvider(
                  username='scylla',
                  password='XOEDTTBPZGYAZIQD')

cluster = Cluster(
            contact_points = ["aws-us-east-1-portal9.dblayer.com"],
            port = 15401,
            auth_provider = auth_provider)

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
