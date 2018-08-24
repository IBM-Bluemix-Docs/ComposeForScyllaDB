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

# Usando cqlsh
{: #using-cqlsh}

É possível usar `cqlsh` para se conectar diretamente ao {{site.data.keyword.composeForScyllaDB_full}}.

Desde a gravação, `cqlsh` é um programa Python 2.7 e não é executado com o Python 3.
{: .tip}

Como `cqlsh` é um aplicativo Python, é possível executar `pip install cqlsh` para instalar a versão mais recente do comando.

Também é possível obter `cqlsh` por meio de uma distribuição completa do Cassandra, fazendo download dele de [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/), que oferece código binário, código-fonte e instruções sobre como instalar o Cassandra em distribuições Linux Debian (apt) e Red Hat (RPM). Faça download de uma liberação binária e descompacte o arquivo em um diretório. Então, `cd` nesse diretório e você localizará `cqlsh` no diretório `bin`.

Como obter essa ferramenta em seu dispositivo local depende de seu sistema operacional local. O mais fácil é instalar a liberação mais recente do Cassandra usando um método apropriado para seu sistema operacional (as versões mais recentes ainda suportam a versão 2.1.8, que é o que Scylla é) e, em seguida, usar o comando `cqlsh` integrado. 

Em um Mac, por exemplo, é possível usar `homebrew` para instalar o Cassandra, digitando `brew install cassandra`.

## Arquivos cqlshrc

Quando o comando `cqlsh` é executado, ele carrega `$HOME/.cassandra/cqlshrc` por padrão, o que pode configurar muitos parâmetros que não são configuráveis na linha de comandos. É possível editar o arquivo padrão para configurar parâmetros e quaisquer mudanças feitas se aplicarão a todas as sessões, a menos que sejam substituídas na linha de comandos.

Se você estiver executando múltiplas instâncias do Scylla em ambientes diferentes, será possível criar um arquivo `cqlshrc` separado e utilizá-lo incluindo `--cqlshrc=[filename]` no comando.


Para obter um exemplo de um arquivo cqlshrc completo, consulte a [amostra de cqlshrc do Apache](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). 

### Cqlshrc para o {{site.data.keyword.composeForScyllaDB}}

Um arquivo `cqlshrc` mínimo para se conectar ao seu serviço é semelhante ao seguinte:

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

Insira as informações fornecidas nas _Sequências de conexões_ de seu serviço e salve o arquivo como `example.cqlshrc`.

Agora é possível efetuar login no banco de dados do Scylla executando `cqlsh --ssl --cqlshrc=example.cqlshrc` por meio da linha de comandos. Observe que mesmo que `cqlshrc` tenha uma opção para especificar `ssl=true`, ele parece ser ignorado, portanto, deve-se usar a sinalização `--ssl`.

## TLSv1.2

`cqlsh` não assume o padrão para TLSv1.2 ao negociar uma conexão criptografada. Para se conectar com êxito a uma implementação do Compose Scylla, a versão do TLS deve ser especificada no ambiente que está executando `cqlsh`. Para obter a maior compatibilidade (e um pouco de prova do futuro para o TLSv1.3), configure `SSL_VERSION` no ambiente como SSLv23 usando `export SSL_VERSION=SSLv23` ou colocando `SSL_VERSION=SSLv23` antes do comando de conexão `cqlsh`.

Também é possível configurar a versão especificamente como TLSv1.2 usando `export SSL_VERSION=TLSv1_2` ou colocando `SSL_VERSION=TLSv1_2` antes do comando de conexão `cqlsh`.

## Conectando sem validar

A validação é ativada por padrão, mas é possível se conectar ao Scylla com TLS/SSL sem validar o host remoto. No entanto, isso não é recomendado para sistemas de produção. A validação é controlada pela variável de ambiente `SSL_VALIDATE` ou por configurações SSL no arquivo cqlshrc. Configure `SSL_VALIDATE` como false (`export SSL_VALIDATE=false`) em seu ambiente para desativar a validação.

Como alternativa, é possível editar seu arquivo cqlshrc para desativar a validação.

```
[ssl]  
validate = false
```

Se desejar desativar a validação para um único comando `cqlsh`, inclua `SSL_VALIDATE=false` antes do comando de conexão `cqlsh`. 

Ainda será necessário informar ao `cqlsh` para usar o TLSv1.2, mesmo quando a validação não estiver ativada. Se você não tiver configurado a versão do ssl no ambiente e não quiser validar, a conexão deverá ser semelhante a este exemplo:

```
SSL_VERSION=SSLv23 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtendo e usando o certificado

Se desejar usar cqlsh e verificar o host remoto, precisará obter o certificado, salvá-lo em algum lugar acessível e, em seguida, fornecer o caminho para o certificado para cqlsh.

Configure a variável de ambiente `SSL_CERTFILE` como o caminho e o nome do arquivo de seu certificado salvo para ativá-lo para todas as chamadas subsequentes de `cqlsh`. 

Como alternativa, é possível incluir o seguinte em seu arquivo cqlshrc.

```
[ssl] certfile = ~/path_to_your/lechain.pem validate = true
```

Se você só quiser usar um certificado específico para um único comando `cqlsh` coloque `SSL_CERTFILE = ~/path_to_your/lechain.pem` no início de seu comando de conexão cqlsh. Por exemplo:

```
SSL_VERSION=SSLv23 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Introdução ao cqlsh

Se você digitar `HELP`, será possível ver que o shell tem muita capacidade. Todos os comandos têm até mesmo a conclusão de `TAB`. Por exemplo:

1. Digite `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Você deverá ver as opções para a classe de replicação.
2. Escolha `SimpleStrategy` aqui porque o cluster não ampliará múltiplos data centers.
3. Pressione `<TAB><TAB>` novamente e insira 3 para o valor `replication_factor`. Em seguida, feche a chave com `}` e conclua a instrução com `;<enter>`.

Você criou seu primeiro KEYSPACE e o padronizou para replicar seus dados para os três nós em seu cluster.

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

Agora é possível usar o keyspace:

```sql 
USE my_new_keyspace;
```

E o seu shell agora mostra que o seu prompt de comandos está usando o seu keyspace por padrão. Cada tabela tem que ter um keyspace.

Digite o comando `CREATE TABLE` a seguir no shell para criar uma tabela para preencher com exemplos.

```sql
CREATE TABLE my_new_table (
    my_table_id uuid,
    last_name text,
    first_name text,
    PRIMARY KEY(my_table_id)
);
```
