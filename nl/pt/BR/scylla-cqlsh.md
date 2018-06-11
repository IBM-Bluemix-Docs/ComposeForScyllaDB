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

# Usando cqlsh
{: #using-cqlsh}

É possível se conectar diretamente ao {{site.data.keyword.composeForScyllaDB_full}} usando `cqlsh`.
Há várias maneiras de instalá-lo. Como é um aplicativo Python, executar `pip install cqlsh` instalará a versão mais recente do comando. Desde a gravação, `cqlsh` é um programa Python 2.7 e não é executado com o Python 3.

Também é possível obter `cqlsh` por meio de uma distribuição integral do Cassandra. Isso pode ser obtido por meio de [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) que oferece origem e instruções binárias sobre como instalar distribuições Linux do Cassandra no Debian (apt) e do Red Hat (RPM). Faça download de uma liberação binária e descompacte o arquivo em um diretório. Então, `cd` nesse diretório e você localizará `cqlsh` no diretório `bin`.

Como você obtém essa ferramenta em seu dispositivo local depende de sua plataforma local. O mais fácil é instalar a liberação mais recente do Cassandra usando um método apropriado para sua plataforma (as versões mais recentes ainda suportam a versão 2.1.8, que é o que Scylla é) e usar o comando `cqlsh` integrado. 

Em um Mac, por exemplo, instale o Cassandra com `homebrew` digitando `brew install cassandra`.

## Sobre arquivos cqlshrc
Quando ele é executado, o comando `cqlsh` carrega um arquivo, `$HOME/.cassandra/cqlshrc` por padrão, o qual pode configurar muitos parâmetros que não são configuráveis na linha de comandos. Uma [Amostra do Apache de cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). É possível editar o arquivo padrão para configurar parâmetros. Eles serão aplicados a todas as sessões, a menos que substituídos na linha de comandos.

Se você estiver executando múltiplas instâncias do Scylla em diferentes ambientes, poderá criar um arquivo `cqlshrc` separado e fazer o `cqlsh` usá-lo incluindo `--cqlshrc=[filename]` em seu comando.

### Cqlshrc para o {{site.data.keyword.composeForScyllaDB}}
Este é um arquivo `cqlshrc` mínimo de exemplo para conexão com o seu serviço:
```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl] certfile = ~/path_to_your/lechain.pem validate = true
```

Insira nas informações fornecidas por meio das _Sequências de conexões_ do seu serviço e salve esse arquivo como example.cqlshrc.
Executando `cqlsh --ssl --cqlshrc=example.cqlshrc` por meio da linha de comandos e você será registrado em seu banco de dados do Scylla. Observe que mesmo que `cqlshrc` tenha uma opção para especificar `ssl=true` ele parece ser ignorado de maneira que a sinalização `--ssl` deverá ser usada.

A versão de ssl não pode ser configurada no arquivo `cqlshrc`; portanto, você terá que configurar o ambiente para usar TLSv1.2 ou anexar SSL_VERSION=TLSv1_2 na frente do comando de conexão `cqlsh`.

## TLSv1.2

`cqlsh` não assume o padrão para TLSv1.2 ao negociar uma conexão criptografada. Para se conectar com êxito a uma implementação do Compose Scylla, a versão do TLS deve ser especificada no ambiente que estiver executando `cqlsh`. Configure SSL_VERSION no ambiente usando:
`export SSL_VERSION=TLSv1_2`
ou coloque `SSL_VERSION=TLSv1_2` antes do comando de conexão `cqlsh`.

## Conectando sem validar

A maneira mais simples de se conectar ao Scylla com TLS/SSL é sem validar o host remoto. Isso não é recomendado para produção. Por padrão, a validação está ativada. A validação é controlada pela variável de ambiente `SSL_VALIDATE` ou por configurações SSL no arquivo cqlshrc. Configure `SSL_VALIDATE` como false em seu ambiente para desativar a validação:
`export SSL_VALIDATE=false`

Como alternativa, é possível incluir o seguinte em seu arquivo cqlshrc.

```
[ssl]  
validate = false
```

Se você desejar desativá-lo apenas para um único comando `cqlsh`, coloque `SSL_VALIDATE=false` antes do comando de conexão `cqlsh`. 

Você ainda tem que indicar ao `cqlsh` para usar o TLSv1.2, mesmo quando não validar o certificado. Se você não tiver configurado a versão do ssl no ambiente e não quiser validar, a conexão deverá ser semelhante a este exemplo:

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtendo e usando o certificado

Se você desejar usar cqlsh e verificar o host remoto, precisará obter o certificado, salvá-lo em algum lugar acessível e, em seguida, fornecer o caminho para o certificado para cqlsh. Uma cópia do certificado reside na página [Scylla e certificados](doc:scylla-and-certificates), bem como detalhes sobre os seus conteúdos e a sua origem. Faça um arquivo de certificado ou salve uma cópia localmente. 

Configure a variável de ambiente `SSL_CERTFILE` para o caminho e o nome do arquivo de seu certificado salvo para ativá-lo para todas as chamadas subsequentes de `cqlsh`. 

Como alternativa, você poderia incluir o seguinte em seu arquivo cqlshrc.
```
[ssl] certfile = ~/path_to_your/lechain.pem validate = true
```

Se você só quiser usar um certificado específico para um único comando `cqlsh` coloque `SSL_CERTFILE = ~/path_to_your/lechain.pem` no início de seu comando de conexão cqlsh. Por exemplo:

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Usando cqlsh

Se você digitar `HELP`, será possível ver que o shell tem muita capacidade. O melhor é que todos esses comandos têm a conclusão de `TAB` também. Por exemplo:
1. Digite `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. Você deverá ver as opções para a classe de replicação.
2. É possível escolher `SimpleStrategy` aqui porque o cluster não abrangerá múltiplos data centers.
3. Pressione `<TAB><TAB>` novamente e insira 3 para o replication_factor. Em seguida, feche a chave com `}` e conclua a instrução com `;<enter>`.

Você criou seu primeiro KEYSPACE e o padronizou para replicar seus dados para todos os três nós em seu cluster.
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
