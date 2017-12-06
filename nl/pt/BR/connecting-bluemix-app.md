---

Copyright:
  Years: 2016,2017
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando um aplicativo {{site.data.keyword.cloud_notm}}

Para conectar um aplicativo {{site.data.keyword.cloud}} a seu serviço, use as credenciais de serviço que são criadas quando o serviço é provisionado. O aplicativo de amostra demostra como usar o Node.js para se conectar a um serviço {{site.data.keyword.composeForPostgreSQL_full}} usando as credenciais fornecidas e como criar um banco de dados e ler e gravar no banco de dados.

## Conectando usando o aplicativo de amostra 'Hello World'

O aplicativo de amostra [compose-scylladb-helloworld-nodejs](https://github.com/IBM-Bluemix/compose-scylladb-helloworld-nodejs) demonstra como usar o Node.js para se conectar a um serviço {{site.data.keyword.composeForScyllaDB}} usando as credenciais fornecidas. O aplicativo cria, lê e grava em um banco de dados

Faça download do aplicativo de amostra e siga as instruções no arquivo leia-me. Em seguida, em sua página de detalhes do aplicativo no {{site.data.keyword.cloud_notm}}, clique em **Visualizar app** para visualizar os conteúdos da tabela *exemplos*.

## Credenciais disponíveis

Campo de nome|Descrição
----------|-----------
`db_type`|O tipo de banco de dados que é oferecido pelo serviço, nesse caso `scylla`.
`uri_cli_1`|Uma linha de comandos shell `cqlsh` alternativa que se conecta à instância de banco de dados.
`maps`|Um mapa de conexão do ScyllaDB que fornece as informações que são necessárias por vários drivers para associar endereços IP internos aos nomes DNS externos de um banco de dados ScyllaDB.
`name`|O nome da implementação do banco de dados.
`uri_cli`|Uma linha de comandos shell `cqlsh` que se conecta à instância de banco de dados.
`uri_direct_2`|Um URI alternativo que pode ser usado para se conectar ao serviço. Formatado como para `uri`.
`uri_direct_1`|Um URI alternativo que pode ser usado para se conectar ao serviço. Formatado como para `uri`.
`ca_certificate_base64`|Um certificado autoassinado que é usado para confirmar se um aplicativo está se conectando ao servidor apropriado. O certificado é codificado em base64.
`deployment_id`|Um identificador interno para o serviço conforme criado no Compose.
`uri_cli_2`|Uma linha de comandos shell `cqlsh` alternativa que se conecta à instância de banco de dados.
`uri`|O URI que é usado ao se conectar ao serviço, que inclui o esquema (`scylla:`), a senha, o nome do host do servidor, o número da porta à qual se conectar e o nome do banco de dados.
{: caption="Tabela 1. Credenciais do Compose for ScyllaDB" caption-side="top"}
