---

copyright:
  years: 2016,2018
lastupdated: "2018-05-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando um aplicativo {{site.data.keyword.cloud_notm}}

Para conectar um aplicativo {{site.data.keyword.cloud}} a seu serviço, use as credenciais de serviço que são criadas quando o serviço é provisionado.

## Conectando ao app de amostra 'Hello World'

O app de amostra [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Cloud/compose-scylladb-helloworld-nodejs) demonstra como usar Node.js para se conectar a um serviço {{site.data.keyword.composeForScyllaDB}} usando as credenciais fornecidas. O aplicativo cria, lê e grava em um banco de dados.

Faça download do aplicativo de amostra e siga as instruções no arquivo leia-me. Em seguida, em sua página de detalhes do aplicativo no {{site.data.keyword.cloud_notm}}, clique em **Visualizar app** para visualizar os conteúdos da tabela *exemplos*.

## Credenciais disponíveis

Campo de nome|Descrição
----------|-----------
`db_type`|O tipo de banco de dados que é oferecido pelo serviço, nesse caso `scylla`.
`uri_cli_1`|Uma linha de comandos shell `cqlsh` secundária que se conecta à instância de banco de dados.
`maps`|Um mapa de conexão do ScyllaDB que fornece as informações que são necessárias por vários drivers para associar endereços IP internos aos nomes DNS externos de um banco de dados ScyllaDB.
`name`|O nome da implementação do banco de dados.
`uri_cli`|Uma linha de comandos shell `cqlsh` que se conecta à instância de banco de dados.
`uri_direct_2`|Um terceiro URI que pode ser usado para se conectar ao serviço. O formato é o mesmo que `uri`.
`uri_direct_1`|Um URI secundário que pode ser usado para se conectar ao serviço. O formato é o mesmo que `uri`.
` ca_certificate_base64 `  ` (opcional) `|Um certificado autoassinado codificado em base64 usado para confirmar se um aplicativo está se conectando ao servidor apropriado. O certificado está presente apenas em serviços que possuem um certificado autoassinado em vez de um certificado Let's Encrypt. É necessário decodificar a chave antes de poder usá-la, conforme mostrado no aplicativo de amostra.
`deployment_id`|Um identificador interno para o serviço conforme criado no Compose.
`uri_cli_2`|Uma terceira linha de comandos shell `cqlsh` que se conecta à instância de banco de dados.
`uri`|O URI que é usado ao se conectar ao serviço, que inclui o esquema (`scylla:`), a senha, o nome do host do servidor, o número da porta à qual se conectar e o nome do banco de dados.
{: caption="Tabela 1. Credenciais do Compose for ScyllaDB" caption-side="top"}

**Nota:** três portais `haproxy` fornecem acesso ao serviço Scylla. Tanto `uri` quanto `uri_direct_1` e `uri_direct_2` podem ser usados para a conexão. Em seus aplicativos, alterne entre `uri`, `uri_direct_1` e `uri_direct_2` para gerenciar respostas para falhas de conexão.
